<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>菜演播厅 - 发酵美食的可视化探索</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/pinia@2.1.7/dist/pinia.iife.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Noto Sans SC', sans-serif;
        }

        :root {
            --primary: #8e44ad;
            --primary-dark: #6c3483;
            --secondary: #16a085;
            --secondary-dark: #117a65;
            --accent: #e67e22;
            --light: #f8f9fa;
            --dark: #2c3e50;
            --gray: #95a5a6;
            --success: #27ae60;
            --danger: #e74c3c;
            --warning: #f39c12;
            --card-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: var(--light);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(44, 62, 80, 0.7);
            border-radius: 16px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            font-weight: 700;
        }

        .subtitle {
            font-size: 1.3rem;
            color: var(--gray);
            max-width: 800px;
            margin: 0 auto;
            line-height: 1.6;
        }

        .main-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            grid-template-rows: auto auto auto;
            gap: 20px;
            grid-template-areas:
                "showcase radar"
                "backtrace mixer"
                "innovation innovation";
        }

        .card {
            background: rgba(44, 62, 80, 0.7);
            border-radius: 16px;
            padding: 20px;
            box-shadow: var(--card-shadow);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: var(--transition);
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.2);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .card-title {
            font-size: 1.5rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .card-title i {
            color: var(--primary);
        }

        .view-toggle {
            display: flex;
            gap: 10px;
        }

        .toggle-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: var(--light);
            padding: 8px 15px;
            border-radius: 8px;
            cursor: pointer;
            transition: var(--transition);
            font-size: 0.9rem;
        }

        .toggle-btn.active {
            background: var(--primary);
        }

        /* Dish Showcase Styles */
        .showcase {
            grid-area: showcase;
            height: 400px;
            display: flex;
            flex-direction: column;
        }

        .dish-container {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .dish-image {
            max-width: 80%;
            max-height: 250px;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            transition: transform 0.5s ease;
        }

        .dish-image.rotating {
            animation: rotate 20s linear infinite;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .tags-container {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-top: 20px;
            justify-content: center;
        }

        .tag {
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 500;
        }

        .tag.type {
            background: rgba(142, 68, 173, 0.3);
            border: 1px solid var(--primary);
        }

        .tag.health {
            background: rgba(22, 160, 133, 0.3);
            border: 1px solid var(--secondary);
        }

        .tag.function {
            background: rgba(231, 126, 34, 0.3);
            border: 1px solid var(--accent);
        }

        /* Radar Chart Styles */
        .radar {
            grid-area: radar;
            height: 400px;
        }

        .chart-container {
            width: 100%;
            height: 350px;
        }

        /* Backtrace Diagram Styles */
        .backtrace {
            grid-area: backtrace;
            height: 400px;
        }

        .diagram-container {
            width: 100%;
            height: 350px;
            position: relative;
        }

        /* Simulate Mixer Styles */
        .mixer {
            grid-area: mixer;
            height: 400px;
        }

        .mixer-ui {
            display: grid;
            grid-template-columns: 150px 1fr;
            gap: 20px;
            height: 350px;
        }

        .ingredients-panel {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 12px;
            padding: 15px;
            overflow-y: auto;
        }

        .ingredient {
            background: rgba(255, 255, 255, 0.1);
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 10px;
            cursor: grab;
            display: flex;
            align-items: center;
            gap: 10px;
            transition: var(--transition);
        }

        .ingredient:hover {
            background: rgba(142, 68, 173, 0.3);
        }

        .ingredient i {
            color: var(--primary);
        }

        .mixer-canvas {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 12px;
            border: 2px dashed rgba(255, 255, 255, 0.2);
            position: relative;
            overflow: hidden;
        }

        .mix-result {
            position: absolute;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            padding: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
            cursor: move;
        }

        /* Innovation Log Styles */
        .innovation {
            grid-area: innovation;
            min-height: 300px;
        }

        .logs-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .log-card {
            background: rgba(44, 62, 80, 0.8);
            border-radius: 12px;
            padding: 15px;
            transition: var(--transition);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .log-card:hover {
            transform: translateY(-5px);
            border-color: var(--primary);
        }

        .log-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
        }

        .log-name {
            font-weight: 600;
            font-size: 1.1rem;
        }

        .log-status {
            padding: 3px 8px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
        }

        .status-draft {
            background: rgba(243, 156, 18, 0.3);
            border: 1px solid var(--warning);
        }

        .status-submitted {
            background: rgba(39, 174, 96, 0.3);
            border: 1px solid var(--success);
        }

        .log-content {
            color: var(--gray);
            font-size: 0.9rem;
            margin-bottom: 15px;
            line-height: 1.5;
        }

        .log-tags {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }

        .microbe-tag {
            background: rgba(142, 68, 173, 0.2);
            padding: 4px 8px;
            border-radius: 20px;
            font-size: 0.8rem;
        }

        .actions {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }

        .btn {
            padding: 8px 15px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: 500;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .btn:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }

        .tooltip {
            position: absolute;
            background: rgba(44, 62, 80, 0.95);
            border: 1px solid var(--primary);
            border-radius: 8px;
            padding: 12px;
            z-index: 100;
            max-width: 300px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
        }

        .tooltip-title {
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--primary);
        }

        .tooltip-content {
            font-size: 0.9rem;
            line-height: 1.4;
        }

        .tooltip-microbes {
            margin-top: 10px;
            padding-top: 10px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .microbe-item {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
            font-size: 0.85rem;
        }

        /* Responsive Design */
        @media (max-width: 1024px) {
            .main-grid {
                grid-template-columns: 1fr;
                grid-template-areas:
                    "showcase"
                    "radar"
                    "backtrace"
                    "mixer"
                    "innovation";
            }
        }

        @media (max-width: 768px) {
            h1 {
                font-size: 2.2rem;
            }
            
            .subtitle {
                font-size: 1.1rem;
            }
            
            .logs-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div id="app">
        <div class="container">
            <header>
                <h1><i class="fas fa-microscope"></i> 菜演播厅</h1>
                <p class="subtitle">从一道菜的风味出发，反推发酵智慧与菌群搭配艺术</p>
            </header>

            <div class="main-grid">
                <!-- 成菜演播视图 -->
                <section class="card showcase">
                    <div class="card-header">
                        <h2 class="card-title"><i class="fas fa-utensils"></i> 成菜演播视图</h2>
                        <div class="view-toggle">
                            <button class="toggle-btn" :class="{'active': viewMode === 'card'}" @click="viewMode = 'card'">卡片模式</button>
                            <button class="toggle-btn" :class="{'active': viewMode === 'immersive'}" @click="viewMode = 'immersive'">全景模式</button>
                        </div>
                    </div>
                    <div class="dish-container">
                        <img :src="currentDish.image" alt="当前菜品" class="dish-image" :class="{'rotating': isRotating}">
                        <button class="btn btn-secondary" @click="toggleRotation" style="position: absolute; bottom: 10px; right: 10px;">
                            <i class="fas" :class="isRotating ? 'fa-pause' : 'fa-sync-alt'"></i>
                        </button>
                    </div>
                    <div class="tags-container">
                        <div class="tag type">酸辣味型</div>
                        <div class="tag health">建议午餐食用</div>
                        <div class="tag function">促进消化</div>
                        <div class="tag health">低盐配方</div>
                        <div class="tag function">益生菌丰富</div>
                    </div>
                </section>

                <!-- 味型结构雷达图 -->
                <section class="card radar">
                    <div class="card-header">
                        <h2 class="card-title"><i class="fas fa-chart-radar"></i> 味型结构雷达图</h2>
                    </div>
                    <div class="chart-container" ref="radarChart"></div>
                </section>

                <!-- 反推调味图谱 -->
                <section class="card backtrace">
                    <div class="card-header">
                        <h2 class="card-title"><i class="fas fa-project-diagram"></i> 反推调味图谱</h2>
                    </div>
                    <div class="diagram-container" ref="backtraceDiagram"></div>
                </section>

                <!-- 调味配器交互台 -->
                <section class="card mixer">
                    <div class="card-header">
                        <h2 class="card-title"><i class="fas fa-blender"></i> 调味配器交互台</h2>
                    </div>
                    <div class="mixer-ui">
                        <div class="ingredients-panel">
                            <h3 style="margin-bottom: 15px; font-size: 1.1rem;">配料库</h3>
                            <div class="ingredient" draggable="true" @dragstart="dragStart($event, ingredient)" v-for="ingredient in ingredients" :key="ingredient.id">
                                <i :class="ingredient.icon"></i>
                                <span>{{ ingredient.name }}</span>
                            </div>
                        </div>
                        <div class="mixer-canvas" @dragover.prevent @drop="dropIngredient">
                            <div class="mix-result" v-for="(item, index) in mixedItems" :key="index"
                                :style="{ left: item.x + 'px', top: item.y + 'px' }"
                                draggable="true"
                                @dragstart="dragMixItem($event, index)"
                                @dragend="dragEnd">
                                <i :class="item.ingredient.icon"></i>
                                <span>{{ item.ingredient.name }}</span>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- 创新组合记录器 -->
                <section class="card innovation">
                    <div class="card-header">
                        <h2 class="card-title"><i class="fas fa-lightbulb"></i> 创新组合记录器</h2>
                        <div>
                            <button class="btn btn-primary" @click="saveDraft">
                                <i class="fas fa-save"></i> 保存当前配方
                            </button>
                        </div>
                    </div>
                    <div class="logs-container">
                        <div class="log-card" v-for="(draft, index) in drafts" :key="index">
                            <div class="log-header">
                                <div class="log-name">{{ draft.name }}</div>
                                <div class="log-status" :class="'status-' + draft.status">{{ draft.status === 'draft' ? '草稿中' : '已提交' }}</div>
                            </div>
                            <div class="log-content">
                                {{ draft.description }}
                            </div>
                            <div class="log-tags">
                                <div class="microbe-tag" v-for="(microbe, mIndex) in draft.microbes" :key="mIndex">
                                    {{ microbe }}
                                </div>
                            </div>
                            <div class="actions">
                                <button class="btn btn-primary">
                                    <i class="fas fa-edit"></i> 编辑
                                </button>
                                <button class="btn btn-secondary">
                                    <i class="fas fa-share-alt"></i> 分享
                                </button>
                            </div>
                        </div>
                    </div>
                </section>
            </div>
        </div>

        <!-- Tooltip -->
        <div class="tooltip" v-if="tooltip.visible" :style="{ left: tooltip.x + 'px', top: tooltip.y + 'px' }">
            <div class="tooltip-title">{{ tooltip.title }}</div>
            <div class="tooltip-content">{{ tooltip.content }}</div>
            <div class="tooltip-microbes">
                <div class="microbe-item" v-for="(microbe, index) in tooltip.microbes" :key="index">
                    <i class="fas fa-bacteria"></i> {{ microbe }}
                </div>
            </div>
        </div>
    </div>

    <script>
        const { createApp, ref, onMounted, watch } = Vue;
        const { createPinia, defineStore } = Pinia;
        
        // 创建 Pinia 实例
        const pinia = createPinia();
        
        // 定义风味存储
        const useFlavorStore = defineStore('flavor', {
            state: () => ({
                flavors: [],
                selected: null,
                drafts: [],
            }),
            actions: {
                addFlavor(f) {
                    this.flavors.push(f);
                },
                selectFlavor(f) {
                    this.selected = f;
                },
                saveDraft(draft) {
                    draft.id = Date.now();
                    this.drafts.unshift(draft);
                }
            }
        });
        
        const app = createApp({
            setup() {
                // 使用 Pinia store
                const flavorStore = useFlavorStore();
                
                // 响应式数据
                const viewMode = ref('card');
                const isRotating = ref(false);
                const radarChart = ref(null);
                const backtraceDiagram = ref(null);
                const currentDish = ref({
                    id: 'dish-001',
                    name: '川式泡菜豆腐',
                    image: 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=400&q=80',
                    tasteProfile: {
                        sour: 7,
                        sweet: 3,
                        bitter: 1,
                        salty: 5,
                        spicy: 8,
                        umami: 6,
                        freshness: 9
                    },
                    healthFunction: ['助消化', '益生菌'],
                    recommendTime: '午餐',
                    mainIngredients: ['豆腐', '泡菜', '辣椒'],
                    fermentedCondiments: [
                        {
                            id: 'cond-001',
                            name: '四川泡菜',
                            rawMaterial: '白菜、萝卜、辣椒',
                            fermentation: {
                                strain: '乳酸菌',
                                duration: '15天',
                                environment: '厌氧',
                                temperature: '18-22°C'
                            }
                        }
                    ]
                });
                
                const ingredients = ref([
                    { id: 'ing-1', name: '泡菜', icon: 'fas fa-carrot', type: 'fermented' },
                    { id: 'ing-2', name: '豆腐', icon: 'fas fa-cheese', type: 'main' },
                    { id: 'ing-3', name: '辣椒酱', icon: 'fas fa-pepper-hot', type: 'fermented' },
                    { id: 'ing-4', name: '味噌', icon: 'fas fa-jar', type: 'fermented' },
                    { id: 'ing-5', name: '酱油', icon: 'fas fa-flask', type: 'fermented' },
                    { id: 'ing-6', name: '盐', icon: 'fas fa-mortar-pestle', type: 'basic' },
                    { id: 'ing-7', name: '糖', icon: 'fas fa-cookie', type: 'basic' },
                    { id: 'ing-8', name: '醋', icon: 'fas fa-wine-bottle', type: 'basic' }
                ]);
                
                const mixedItems = ref([]);
                const draggedItem = ref(null);
                const tooltip = ref({
                    visible: false,
                    x: 0,
                    y: 0,
                    title: '',
                    content: '',
                    microbes: []
                });
                
                const drafts = ref([
                    {
                        id: 'draft-001',
                        name: '凉拌三色豆腐乳拼盘',
                        description: '结合四川泡菜、广式腐乳和日式味噌的创新组合',
                        status: 'draft',
                        microbes: ['乳酸菌', '米曲霉', '毛霉菌']
                    },
                    {
                        id: 'draft-002',
                        name: '韩式泡菜豆腐锅',
                        description: '传统韩式泡菜与嫩豆腐的完美结合',
                        status: 'submitted',
                        microbes: ['乳酸菌', '芽孢杆菌']
                    },
                    {
                        id: 'draft-003',
                        name: '味噌烤茄子',
                        description: '日式味噌与地中海烤茄子的创新融合',
                        status: 'draft',
                        microbes: ['米曲霉', '酵母菌']
                    }
                ]);
                
                // 方法
                const toggleRotation = () => {
                    isRotating.value = !isRotating.value;
                };
                
                const dragStart = (event, ingredient) => {
                    draggedItem.value = ingredient;
                    event.dataTransfer.setData('text/plain', ingredient.id);
                };
                
                const dropIngredient = (event) => {
                    if (draggedItem.value) {
                        const rect = event.currentTarget.getBoundingClientRect();
                        const x = event.clientX - rect.left - 30;
                        const y = event.clientY - rect.top - 20;
                        
                        mixedItems.value.push({
                            ingredient: draggedItem.value,
                            x: x,
                            y: y
                        });
                        
                        draggedItem.value = null;
                    }
                };
                
                const dragMixItem = (event, index) => {
                    event.dataTransfer.setData('text/plain', index.toString());
                };
                
                const dragEnd = () => {
                    // 拖动结束处理
                };
                
                const showTooltip = (event, title, content, microbes) => {
                    tooltip.value = {
                        visible: true,
                        x: event.clientX + 15,
                        y: event.clientY + 15,
                        title: title,
                        content: content,
                        microbes: microbes
                    };
                };
                
                const hideTooltip = () => {
                    tooltip.value.visible = false;
                };
                
                const saveDraft = () => {
                    const newDraft = {
                        name: '创新配方 #' + (drafts.value.length + 1),
                        description: '基于' + mixedItems.value.map(item => item.ingredient.name).join(' + ') + '的创新组合',
                        status: 'draft',
                        microbes: ['乳酸菌', '酵母菌']
                    };
                    
                    flavorStore.saveDraft(newDraft);
                    drafts.value.unshift(newDraft);
                    
                    alert('配方草稿已保存！');
                };
                
                // 初始化图表
                onMounted(() => {
                    // 初始化雷达图
                    const radarChartInstance = echarts.init(radarChart.value);
                    const radarOption = {
                        title: {
                            text: '风味分析',
                            textStyle: { color: '#fff' }
                        },
                        tooltip: {
                            trigger: 'item',
                            formatter: function(params) {
                                return params.name + ': ' + params.value;
                            }
                        },
                        legend: {
                            data: ['当前菜品'],
                            textStyle: { color: '#fff' }
                        },
                        radar: {
                            indicator: [
                                { name: '酸度', max: 10 },
                                { name: '甜度', max: 10 },
                                { name: '苦味', max: 10 },
                                { name: '咸度', max: 10 },
                                { name: '辣度', max: 10 },
                                { name: '鲜味', max: 10 },
                                { name: '清爽度', max: 10 }
                            ],
                            shape: 'circle',
                            splitNumber: 5,
                            axisName: {
                                color: '#fff',
                                backgroundColor: '#333',
                                borderRadius: 3,
                                padding: [3, 5]
                            },
                            splitLine: {
                                lineStyle: {
                                    color: 'rgba(255, 255, 255, 0.1)'
                                }
                            },
                            splitArea: {
                                show: true,
                                areaStyle: {
                                    color: ['rgba(44, 62, 80, 0.5)', 'rgba(44, 62, 80, 0.3)']
                                }
                            },
                            axisLine: {
                                lineStyle: {
                                    color: 'rgba(255, 255, 255, 0.2)'
                                }
                            }
                        },
                        series: [{
                            name: '风味分析',
                            type: 'radar',
                            data: [{
                                value: [
                                    currentDish.value.tasteProfile.sour,
                                    currentDish.value.tasteProfile.sweet,
                                    currentDish.value.tasteProfile.bitter,
                                    currentDish.value.tasteProfile.salty,
                                    currentDish.value.tasteProfile.spicy,
                                    currentDish.value.tasteProfile.umami,
                                    currentDish.value.tasteProfile.freshness
                                ],
                                name: '当前菜品',
                                areaStyle: {
                                    color: new echarts.graphic.RadialGradient(0.5, 0.5, 1, [{
                                        offset: 0,
                                        color: 'rgba(142, 68, 173, 0.5)'
                                    }, {
                                        offset: 1,
                                        color: 'rgba(142, 68, 173, 0.1)'
                                    }])
                                },
                                lineStyle: {
                                    color: 'rgba(142, 68, 173, 1)'
                                },
                                itemStyle: {
                                    color: 'rgba(142, 68, 173, 1)'
                                }
                            }]
                        }]
                    };
                    radarChartInstance.setOption(radarOption);
                    
                    // 初始化反推图谱
                    const diagramInstance = echarts.init(backtraceDiagram.value);
                    const diagramOption = {
                        title: {
                            text: '发酵链路图谱',
                            textStyle: { color: '#fff' }
                        },
                        tooltip: {
                            trigger: 'item',
                            formatter: function(params) {
                                return params.name;
                            }
                        },
                        animationDurationUpdate: 1500,
                        animationEasingUpdate: 'quinticInOut',
                        series: [{
                            type: 'graph',
                            layout: 'circular',
                            symbolSize: 60,
                            roam: true,
                            focusNodeAdjacency: true,
                            label: {
                                show: true,
                                position: 'inside',
                                formatter: '{b}',
                                fontSize: 12,
                                color: '#fff'
                            },
                            edgeSymbol: ['circle', 'arrow'],
                            edgeSymbolSize: [4, 10],
                            edgeLabel: {
                                fontSize: 12,
                                color: '#fff'
                            },
                            data: [{
                                name: '四川泡菜',
                                category: 0,
                                symbolSize: 50,
                                itemStyle: {
                                    color: '#8e44ad'
                                }
                            }, {
                                name: '辣椒酱',
                                category: 0,
                                symbolSize: 50,
                                itemStyle: {
                                    color: '#e74c3c'
                                }
                            }, {
                                name: '豆腐',
                                category: 1,
                                symbolSize: 60,
                                itemStyle: {
                                    color: '#3498db'
                                }
                            }, {
                                name: '盐',
                                category: 2,
                                symbolSize: 40,
                                itemStyle: {
                                    color: '#ecf0f1'
                                }
                            }, {
                                name: '乳酸菌',
                                category: 3,
                                symbolSize: 40,
                                itemStyle: {
                                    color: '#2ecc71'
                                }
                            }, {
                                name: '米曲霉',
                                category: 3,
                                symbolSize: 40,
                                itemStyle: {
                                    color: '#2ecc71'
                                }
                            }, {
                                name: '川式泡菜豆腐',
                                category: 4,
                                symbolSize: 70,
                                itemStyle: {
                                    color: '#e67e22'
                                }
                            }],
                            links: [{
                                source: '四川泡菜',
                                target: '川式泡菜豆腐',
                                lineStyle: {
                                    width: 2,
                                    color: '#8e44ad'
                                }
                            }, {
                                source: '辣椒酱',
                                target: '川式泡菜豆腐',
                                lineStyle: {
                                    width: 2,
                                    color: '#e74c3c'
                                }
                            }, {
                                source: '豆腐',
                                target: '川式泡菜豆腐',
                                lineStyle: {
                                    width: 2,
                                    color: '#3498db'
                                }
                            }, {
                                source: '盐',
                                target: '四川泡菜',
                                lineStyle: {
                                    width: 1,
                                    color: '#ecf0f1'
                                }
                            }, {
                                source: '乳酸菌',
                                target: '四川泡菜',
                                lineStyle: {
                                    width: 1,
                                    color: '#2ecc71'
                                }
                            }, {
                                source: '米曲霉',
                                target: '辣椒酱',
                                lineStyle: {
                                    width: 1,
                                    color: '#2ecc71'
                                }
                            }],
                            categories: [{
                                name: '发酵调味料'
                            }, {
                                name: '主食材'
                            }, {
                                name: '基础调味'
                            }, {
                                name: '菌种'
                            }, {
                                name: '成菜'
                            }],
                            lineStyle: {
                                opacity: 0.9,
                                width: 2,
                                curveness: 0
                            }
                        }]
                    };
                    diagramInstance.setOption(diagramOption);
                    
                    // 添加事件监听器隐藏tooltip
                    document.addEventListener('mousemove', (event) => {
                        if (!tooltip.value.visible) return;
                        tooltip.value.x = event.clientX + 15;
                        tooltip.value.y = event.clientY + 15;
                    });
                    
                    document.addEventListener('click', hideTooltip);
                });
                
                return {
                    viewMode,
                    isRotating,
                    radarChart,
                    backtraceDiagram,
                    currentDish,
                    ingredients,
                    mixedItems,
                    tooltip,
                    drafts,
                    toggleRotation,
                    dragStart,
                    dropIngredient,
                    dragMixItem,
                    dragEnd,
                    showTooltip,
                    hideTooltip,
                    saveDraft
                };
            }
        });
        
        app.use(pinia);
        app.mount('#app');
    </script>
</body>
</html>
