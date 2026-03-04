<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AIRP 灵感生成器 - 6.0 旗舰版</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=Noto+Sans+SC:wght@300;500;700&display=swap');
        
        :root {
            --bg-color: #0a0c14;
            --accent-blue: #3b82f6;
            --accent-purple: #8b5cf6;
            --accent-emerald: #10b981;
            --card-bg: rgba(23, 28, 41, 0.7);
        }

        body {
            font-family: 'Inter', 'Noto Sans SC', sans-serif;
            background: radial-gradient(circle at top right, #1e1b4b, #0a0c10);
            color: #e2e8f0;
            overflow-x: hidden;
        }

        .glass-panel {
            background: var(--card-bg);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.8);
        }

        .category-chip {
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
        }

        .category-chip.active {
            transform: translateY(-4px);
            box-shadow: 0 10px 20px -5px rgba(59, 130, 246, 0.4);
            border: 2px solid var(--accent-blue);
        }

        .tag-pill {
            background: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.2);
            padding: 4px 12px;
            border-radius: 8px;
            font-size: 0.875rem;
            display: inline-block;
            margin: 4px;
            animation: fadeIn 0.4s ease forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.9); }
            to { opacity: 1; transform: scale(1); }
        }

        .roll-btn {
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.2s;
        }

        .roll-btn:hover { transform: rotate(15deg); background: var(--accent-blue); color: white; }

        .master-roll-btn {
            background: linear-gradient(90deg, #3b82f6, #8b5cf6);
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
        }
    </style>
</head>
<body class="min-h-screen py-10 px-4">

    <div class="max-w-6xl mx-auto">
        <header class="text-center mb-10">
            <h1 class="text-5xl font-black tracking-tighter mb-4 italic">
                <span class="bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-emerald-400">AIRP</span> MASTER V6.0
            </h1>
            <p class="text-slate-400 uppercase tracking-widest text-sm italic">全球深度灵感数据库 - 专业功能性模块上线</p>
        </header>

        <!-- 一级分类标签 -->
        <div class="grid grid-cols-2 md:grid-cols-6 gap-3 mb-10">
            <button onclick="setCategory('IP_REAL', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2">
                <span class="text-2xl">📽️</span>
                <span class="font-bold text-xs">IP 真人风</span>
            </button>
            <button onclick="setCategory('IP_ANIME', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2">
                <span class="text-2xl">🎮</span>
                <span class="font-bold text-xs">IP 二次元</span>
            </button>
            <button onclick="setCategory('STORY_DRAMA', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2">
                <span class="text-2xl">⚔️</span>
                <span class="font-bold text-xs">剧情正剧</span>
            </button>
            <button onclick="setCategory('STORY_ROMANCE', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2">
                <span class="text-2xl">🖤</span>
                <span class="font-bold text-xs">陪伴言情</span>
            </button>
            <button onclick="setCategory('FUNC_PRO', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2 border-emerald-500/30">
                <span class="text-2xl">💼</span>
                <span class="font-bold text-xs text-emerald-400">专业/生活</span>
            </button>
            <button onclick="setCategory('FUNC_MEME', this)" class="category-chip glass-panel p-4 rounded-2xl flex flex-col items-center gap-2">
                <span class="text-2xl">🤪</span>
                <span class="font-bold text-xs">搞怪/猎奇</span>
            </button>
        </div>

        <!-- 随机生成主工作区 -->
        <div id="workspace" class="hidden space-y-6">
            <div class="flex justify-center mb-6">
                <button onclick="generateAll()" class="master-roll-btn px-10 py-3 rounded-full font-black text-lg tracking-widest hover:scale-105 transition-all text-white">
                    全盘重组灵感 (ROLL ALL)
                </button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- 身份/主体 -->
                <div class="slot-card glass-panel p-6 rounded-3xl">
                    <div class="flex justify-between items-center mb-4">
                        <label id="cat-label" class="text-xs font-bold text-slate-500 uppercase">SUBJECT IDENTITY (角色主体)</label>
                        <button onclick="roll('character')" class="roll-btn w-8 h-8 flex items-center justify-center rounded-lg">🔄</button>
                    </div>
                    <div id="slot-character" class="text-2xl font-black text-white min-h-[40px]"></div>
                </div>

                <!-- 性格/人设 -->
                <div class="slot-card glass-panel p-6 rounded-3xl">
                    <div class="flex justify-between items-center mb-4">
                        <label class="text-xs font-bold text-slate-500 uppercase">VIBE & PERSONALITY (性格人设)</label>
                        <button onclick="rollPersonality()" class="roll-btn w-8 h-8 flex items-center justify-center rounded-lg">🔄</button>
                    </div>
                    <div id="slot-personality" class="flex flex-wrap gap-1 min-h-[40px]"></div>
                </div>

                <!-- 情节词/互动 -->
                <div class="slot-card glass-panel p-6 rounded-3xl">
                    <div class="flex justify-between items-center mb-4">
                        <label class="text-xs font-bold text-slate-500 uppercase">INTERACTION TAGS (情节/互动词)</label>
                        <button onclick="rollPlot()" class="roll-btn w-8 h-8 flex items-center justify-center rounded-lg">🔄</button>
                    </div>
                    <div id="slot-plot" class="flex flex-wrap gap-1 min-h-[40px]"></div>
                </div>

                <!-- 场景/环境 -->
                <div class="slot-card glass-panel p-6 rounded-3xl">
                    <div class="flex justify-between items-center mb-4">
                        <label class="text-xs font-bold text-slate-500 uppercase">SCENE CONTEXT (背景/世界观)</label>
                        <button onclick="rollScenario()" class="roll-btn w-8 h-8 flex items-center justify-center rounded-lg">🔄</button>
                    </div>
                    <div id="slot-scenario" class="flex flex-wrap gap-1 min-h-[40px]"></div>
                </div>
            </div>

            <div class="text-center pt-8">
                <button onclick="copyToClipboard()" id="copy-btn" class="px-12 py-4 bg-white text-black rounded-2xl font-bold hover:bg-emerald-400 transition-all">
                    复制提示词组合
                </button>
            </div>
        </div>
    </div>

    <script>
        let currentCategory = '';
        const DB = {
            character: {
                IP_REAL: ["Tommy Shelby (谢尔比 - 浴血黑帮)", "Daemon Targaryen (戴蒙 - 龙之家族)", "Lucifer Morningstar (路西法)", "Astarion (阿斯代伦 - 博德之门3)", "Ghost (幽灵 - COD)", "Leon S. Kennedy (里昂 - 生化危机)"],
                IP_ANIME: ["Gojo Satoru (五条悟)", "Levi Ackerman (利威尔)", "Zhongli (钟离)", "Kafka (卡芙卡)", "Sephiroth (萨菲罗斯)", "Makima (玛奇玛)"],
                STORY_DRAMA: ["Detective (资深侦探)", "Commander (前线指挥官)", "Survivalist (末世幸存者)", "Double Agent (双面间谍)", "Fallen Monarch (废黜的君主)"],
                STORY_ROMANCE: ["Chaebol Heir (偏执财阀继承人)", "Dark Hitman (受重伤的黑帮杀手)", "Shadow Guard (忠犬影子护卫)", "Forbidden Professor (禁欲系教授)"],
                FUNC_PRO: [
                    "Criminal Defense Lawyer (刑事辩护律师)", 
                    "Relationship Therapist (情感咨询师)", 
                    "Elite Personal Trainer (顶奢健身教练)", 
                    "Medical Consultant (私人医学顾问)", 
                    "Financial Advisor (毒舌金融顾问)", 
                    "Style/Image Consultant (形象设计专家)", 
                    "Language Expert (同声传译私教)",
                    "Career Coach (职业生涯导师)",
                    "Interior Designer (室内空间设计师)",
                    "Chef de Cuisine (严苛的法餐总厨)"
                ],
                FUNC_MEME: ["Sassy AI (毒舌人工智能)", "Disappointed Cat (失望的真猫)", "Screaming Chicken (尖叫鸡)", "Untitled Goose (捣蛋鹅)", "Yandere Toaster (病娇烤面包机)"]
            },
            personality: {
                IP_REAL: ["#Possessive (占有欲)", "#Stoic (禁欲)", "#Protective (护短)"],
                IP_ANIME: ["#Tsundere (傲娇)", "#Yandere (病娇)", "#Kuudere (高冷)"],
                STORY_DRAMA: ["#Cynical (毒舌)", "#Ruthless (无情)", "#Strategic (睿智)"],
                STORY_ROMANCE: ["#Obsessive (偏执)", "#Toxic (毒性关系)", "#Gentle (温柔)"],
                FUNC_PRO: [
                    "#Professional (极度专业)", 
                    "#Rational (理智到冷血)", 
                    "#Perfectionist (完美主义)", 
                    "#Judgmental (犀利审视)", 
                    "#Encouraging (励志鼓舞)",
                    "#Sophisticated (成熟洗练)"
                ],
                FUNC_MEME: ["#Chaotic (混乱)", "#Sarcastic (讽刺)", "#Chill (佛系)"]
            },
            plot: {
                STORY_ROMANCE: [["#Rainy Night (雨夜互助)", "#In The Car (密闭车内)", "#Enemies to Lovers (宿敌变爱人)"]],
                FUNC_PRO: [
                    ["#Legal Case (卷宗分析)", "#Cross Examination (庭审模拟)", "#Compliance Check (合规性检查)"],
                    ["#Intensive Workout (地狱训练)", "#Meal Plan (饮食监督)", "#Physique Check (身材监测)"],
                    ["#Therapy Session (心理咨询)", "#Vulnerability (展露软肋)", "#Safe Space (治愈避风港)"],
                    ["#Outfit Rating (穿搭评分)", "#Red Carpet Prep (晚宴准备)", "#Color Theory (色彩美学)"],
                    ["#Interview Mock (模拟面试)", "#Salary Negotiation (薪资谈判)", "#Public Speaking (演讲特训)"]
                ],
                FUNC_MEME: [["#TruthBomb (真相痛击)", "#Roast (疯狂吐槽)", "#Chaos (纯粹混乱)"]],
                STORY_DRAMA: [["#Political Intrigue (权谋)", "#Survival (求生)", "#Mystery (解谜)"]]
            },
            scenario: {
                FUNC_PRO: ["Corporate HQ (总部顶层)", "Private Studio (私人工作室)", "High-end Clinic (高端诊所)", "Judicial Court (法庭庭审)", "Library (私人藏书馆)"],
                GENERAL: ["Neon Alley (霓虹巷)", "Mist Forest (雾林)", "Penthouse (豪宅)", "Interrogation Room (审讯室)"]
            }
        };

        function setCategory(cat, el) {
            currentCategory = cat;
            document.querySelectorAll('.category-chip').forEach(b => b.classList.remove('active'));
            el.classList.add('active');
            document.getElementById('workspace').classList.remove('hidden');
            generateAll();
        }

        function roll(type) {
            const el = document.getElementById(`slot-${type}`);
            const data = DB.character[currentCategory] || DB.character.IP_REAL;
            el.innerText = data[Math.floor(Math.random() * data.length)];
            el.style.animation = 'none'; void el.offsetWidth; el.style.animation = 'fadeIn 0.5s ease';
        }

        function rollPersonality() {
            const container = document.getElementById('slot-personality');
            container.innerHTML = '';
            let pool = DB.personality[currentCategory] || DB.personality.STORY_DRAMA;
            let selected = pool.sort(() => 0.5 - Math.random()).slice(0, 3);
            selected.forEach(tag => {
                const span = document.createElement('span');
                span.className = 'tag-pill';
                span.innerText = tag;
                container.appendChild(span);
            });
        }

        function rollPlot() {
            const container = document.getElementById('slot-plot');
            container.innerHTML = '';
            let pool = DB.plot[currentCategory] || DB.plot.STORY_DRAMA;
            let selectedSet = pool[Math.floor(Math.random() * pool.length)];
            selectedSet.forEach(tag => {
                const span = document.createElement('span');
                span.className = 'tag-pill bg-emerald-500/10 text-emerald-300 border-emerald-500/30';
                span.innerText = tag;
                container.appendChild(span);
            });
        }

        function rollScenario() {
            const container = document.getElementById('slot-scenario');
            container.innerHTML = '';
            let pool = (currentCategory === 'FUNC_PRO') ? DB.scenario.FUNC_PRO : DB.scenario.GENERAL;
            let selected = pool.sort(() => 0.5 - Math.random()).slice(0, 2);
            selected.forEach(item => {
                const span = document.createElement('span');
                span.className = 'tag-pill bg-purple-500/10 text-purple-300 border-purple-500/30';
                span.innerText = item;
                container.appendChild(span);
            });
        }

        function generateAll() {
            roll('character');
            rollPersonality();
            rollPlot();
            rollScenario();
        }

        function copyToClipboard() {
            const char = document.getElementById('slot-character').innerText;
            const pers = Array.from(document.querySelectorAll('#slot-personality .tag-pill')).map(e => e.innerText).join(', ');
            const plot = Array.from(document.querySelectorAll('#slot-plot .tag-pill')).map(e => e.innerText).join(', ');
            const scene = Array.from(document.querySelectorAll('#slot-scenario .tag-pill')).map(e => e.innerText).join(' / ');

            const text = `【角色导出】\n类型：${currentCategory}\n角色：${char}\n性格：${pers}\n情节：${plot}\n背景：${scene}`;
            const temp = document.createElement('textarea');
            temp.value = text; document.body.appendChild(temp);
            temp.select(); document.execCommand('copy'); document.body.removeChild(temp);
            
            const btn = document.getElementById('copy-btn');
            btn.innerText = "复制成功！";
            setTimeout(() => btn.innerText = "复制提示词组合", 1500);
        }
    </script>
</body>
</html>
