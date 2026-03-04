<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AIRP 灵感生成器 - 8.0 终极暴力版</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&family=Noto+Sans+SC:wght@400;700;900&display=swap');
        
        :root {
            --bg-color: #02040a;
            --accent-blue: #2563eb;
            --accent-purple: #7c3aed;
            --accent-rose: #e11d48;
            --accent-emerald: #059669;
            --glass: rgba(15, 23, 42, 0.85);
        }

        body {
            font-family: 'Inter', 'Noto Sans SC', sans-serif;
            background-color: var(--bg-color);
            background-image: 
                radial-gradient(circle at 10% 10%, rgba(37, 99, 235, 0.1) 0%, transparent 40%),
                radial-gradient(circle at 90% 90%, rgba(225, 29, 72, 0.1) 0%, transparent 40%);
            color: #f8fafc;
            overflow-x: hidden;
        }

        .glass-card {
            background: var(--glass);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .category-btn {
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.4s;
        }

        .category-btn.active {
            background: linear-gradient(135deg, #2563eb, #7c3aed);
            border-color: transparent;
            transform: translateY(-4px);
            box-shadow: 0 10px 30px -10px rgba(37, 99, 235, 0.5);
        }

        .tag-badge {
            display: inline-flex;
            align-items: center;
            padding: 6px 14px;
            margin: 4px;
            border-radius: 99px;
            font-size: 0.8rem;
            font-weight: 600;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            animation: popIn 0.3s ease-out;
        }

        @keyframes popIn {
            from { opacity: 0; transform: scale(0.8) translateY(10px); }
            to { opacity: 1; transform: scale(1) translateY(0); }
        }

        .roll-main {
            background: linear-gradient(90deg, #2563eb, #7c3aed, #db2777, #2563eb);
            background-size: 300% 100%;
            animation: flow 5s infinite linear;
        }

        @keyframes flow {
            0% { background-position: 0% 50%; }
            100% { background-position: 100% 50%; }
        }

        .slot-title {
            font-size: 0.65rem;
            letter-spacing: 0.2em;
            color: #94a3b8;
            font-weight: 900;
            text-transform: uppercase;
        }
    </style>
</head>
<body class="min-h-screen p-4 md:p-8">

    <div class="max-w-7xl mx-auto">
        <!-- Header -->
        <header class="text-center mb-12">
            <h1 class="text-6xl md:text-8xl font-black italic tracking-tighter mb-4">
                <span class="bg-clip-text text-transparent bg-gradient-to-r from-blue-500 via-rose-500 to-amber-500">AIRP</span> MASTER
            </h1>
            <div class="inline-block px-4 py-1 bg-white/5 rounded-full border border-white/10">
                <span class="text-xs font-bold tracking-[0.5em] text-blue-400">DATABASE V8.0 | 500+ CONTENT TAGS</span>
            </div>
        </header>

        <!-- 一级菜单 -->
        <div class="grid grid-cols-2 md:grid-cols-6 gap-3 mb-12">
            <button onclick="setCategory('IP_REAL', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center">
                <span class="text-3xl mb-2">🎬</span>
                <span class="text-[10px] font-black">真人写实 IP</span>
            </button>
            <button onclick="setCategory('IP_ANIME', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center">
                <span class="text-3xl mb-2">🏮</span>
                <span class="text-[10px] font-black">二次元/游戏 IP</span>
            </button>
            <button onclick="setCategory('DRAMA', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center">
                <span class="text-3xl mb-2">⚔️</span>
                <span class="text-[10px] font-black">剧情/大世界</span>
            </button>
            <button onclick="setCategory('ROMANCE', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center">
                <span class="text-3xl mb-2">💍</span>
                <span class="text-[10px] font-black">言情/陪伴</span>
            </button>
            <button onclick="setCategory('FUNC', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center border-emerald-500/30">
                <span class="text-3xl mb-2">💡</span>
                <span class="text-[10px] font-black text-emerald-400">功能/专业</span>
            </button>
            <button onclick="setCategory('MEME', this)" class="category-btn glass-card p-6 rounded-3xl flex flex-col items-center border-rose-500/30">
                <span class="text-3xl mb-2">🤯</span>
                <span class="text-[10px] font-black text-rose-400">搞怪/猎奇</span>
            </button>
        </div>

        <!-- 随机抽卡区 -->
        <div id="deck-view" class="hidden animate-in fade-in duration-700">
            <div class="flex justify-center mb-10">
                <button onclick="rollAll()" class="roll-main px-16 py-5 rounded-2xl font-black text-xl text-white shadow-2xl hover:scale-105 active:scale-95 transition-all">
                    ROLL THE DICE (全随机抽取)
                </button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- Slot: Character -->
                <div class="glass-card p-8 rounded-[2.5rem] relative overflow-hidden group">
                    <div class="flex justify-between items-center mb-6">
                        <span class="slot-title">01 SUBJECT (主体角色)</span>
                        <button onclick="roll('character')" class="w-8 h-8 rounded-full bg-white/5 hover:bg-blue-500 flex items-center justify-center transition-colors">🔄</button>
                    </div>
                    <div id="s-char" class="text-3xl font-black text-white mb-2">---</div>
                    <div id="s-char-sub" class="text-xs font-bold text-blue-500/80">---</div>
                </div>

                <!-- Slot: Personality -->
                <div class="glass-card p-8 rounded-[2.5rem]">
                    <div class="flex justify-between items-center mb-6">
                        <span class="slot-title">02 PERSONA (性格人设)</span>
                        <button onclick="roll('personality')" class="w-8 h-8 rounded-full bg-white/5 hover:bg-purple-500 flex items-center justify-center transition-colors">🔄</button>
                    </div>
                    <div id="s-pers" class="flex flex-wrap gap-1">---</div>
                </div>

                <!-- Slot: Plot -->
                <div class="glass-card p-8 rounded-[2.5rem]">
                    <div class="flex justify-between items-center mb-6">
                        <span class="slot-title">03 TROPE & PLOT (题材互动)</span>
                        <button onclick="roll('plot')" class="w-8 h-8 rounded-full bg-white/5 hover:bg-rose-500 flex items-center justify-center transition-colors">🔄</button>
                    </div>
                    <div id="s-plot" class="flex flex-wrap gap-1">---</div>
                </div>

                <!-- Slot: Context -->
                <div class="glass-card p-8 rounded-[2.5rem]">
                    <div class="flex justify-between items-center mb-6">
                        <span class="slot-title">04 SCENE (背景世界观)</span>
                        <button onclick="roll('scenario')" class="w-8 h-8 rounded-full bg-white/5 hover:bg-amber-500 flex items-center justify-center transition-colors">🔄</button>
                    </div>
                    <div id="s-scen" class="flex flex-wrap gap-1">---</div>
                </div>
            </div>

            <div class="mt-12 text-center">
                <button onclick="copyIdea()" class="px-12 py-4 bg-white text-black font-black rounded-2xl hover:bg-blue-500 hover:text-white transition-all">
                    复制灵感组合 (COPY ALL)
                </button>
            </div>
        </div>
    </div>

    <script>
        let curCat = '';
        
        // 核心数据库 - 暴力扩充
        const DB = {
            character: {
                IP_REAL: [
                    // 欧美暗黑/Alpha
                    { n: "Tommy Shelby (谢尔比 - 浴血黑帮)", s: "Dark Alpha" },
                    { n: "Daemon Targaryen (戴蒙 - 龙之家族)", s: "Valyrian Blood" },
                    { n: "Lucifer Morningstar (路西法)", s: "Devilishly Charming" },
                    { n: "Astarion (阿斯代伦 - 博德之门3)", s: "Vampire Spawn" },
                    { n: "Ghost / Simon Riley (幽灵 - COD)", s: "Masked Solider" },
                    { n: "Konig (柯尼希 - COD)", s: "Socially Anxious Giant" },
                    { n: "Patrick Bateman (美国精神病人)", s: "Corporate Psycho" },
                    { n: "Tyler Durden (泰勒 - 搏击俱乐部)", s: "Anarchist" },
                    { n: "Hannibal Lecter (汉尼拔)", s: "Culinarian Cannibal" },
                    { n: "Homelander (祖国人)", s: "Villain/Antagonist" },
                    { n: "Massimo Torricelli (365天)", s: "Mafia Boss" },
                    // 完美男友/经典
                    { n: "Steve Rogers (美国队长)", s: "Moral Compass" },
                    { n: "Tom Holland Spider-Man (小蜘蛛)", s: "Boy Next Door" },
                    { n: "Henry Cavill Superman (超人)", s: "Last Son of Krypton" },
                    { n: "Robert Pattinson Batman (蝙蝠侠)", s: "Emo Detective" },
                    { n: "Dean Winchester (迪恩 - 邪恶力量)", s: "Monster Hunter" },
                    { n: "Edward Cullen (爱德华 - 暮光之城)", s: "Sparkling Vampire" },
                    { n: "Anthony Bridgerton (安东尼 - 布里奇顿)", s: "Regency Rake" },
                    { n: "Jamie Fraser (詹米 - 异乡人)", s: "Highlander" },
                    { n: "Mr. Darcy (达西先生)", s: "Austen Hero" },
                    { n: "Sherlock Holmes (神探夏洛克)", s: "High Functioning Sociopath" },
                    { n: "Joel Miller (乔尔 - 最后生还者)", s: "Grizzled Survivor" },
                    { n: "Leon S. Kennedy (里昂 - 生化危机)", s: "Rookie Cop/Agent" },
                    // 大女主
                    { n: "Wanda Maximoff (绯红女巫)", s: "Chaos Magic" },
                    { n: "Wednesday Addams (星期三)", s: "Goth Icon" },
                    { n: "Daenerys Targaryen (龙妈)", s: "Mother of Dragons" },
                    { n: "Villanelle (薇拉内尔 - 嗜血娇娃)", s: "Stylish Assassin" },
                    { n: "Maddy Perez (玛迪 - 亢奋)", s: "It Girl" },
                    // RPF/明星代餐
                    { n: "Harry Styles Archetype (哈卷风)", s: "Soft Rock Star" },
                    { n: "Jungkook Archetype (柾国风)", s: "Golden Maknae" },
                    { n: "Taehyung Archetype (V风)", s: "Artistic Visual" },
                    { n: "Felix Archetype (菲利克斯风)", s: "Deep Voice Angel" },
                    { n: "Timothée Chalamet Archetype (甜茶风)", s: "Ethereal Softboy" },
                    { n: "Pedro Pascal Archetype (佩德罗风)", s: "Internet Daddy" },
                    { n: "Lewis Hamilton Archetype (F1车手风)", s: "Fast & Famous" },
                    // 经典/邪典
                    { n: "Draco Malfoy (德拉科 - 哈利波特)", s: "Slytherin Prince" },
                    { n: "Severus Snape (斯内普)", s: "Tragic Anti-hero" },
                    { n: "Jack Sparrow (杰克船长)", s: "Drunken Pirate" },
                    { n: "Anakin Skywalker (阿纳金)", s: "Chosen One / Vader" },
                    { n: "Legolas (莱戈拉斯)", s: "Elf Archer" },
                    { n: "Ghostface (尖叫狂魔)", s: "Slasher" },
                    { n: "Michael Myers (麦克尔 - 月光光心慌慌)", s: "The Shape" }
                ],
                IP_ANIME: [
                    // 热血/战斗
                    { n: "Gojo Satoru (五条悟)", s: "The Strongest" },
                    { n: "Sukuna (宿傩)", s: "King of Curses" },
                    { n: "Nanami Kento (七海建人)", s: "Overtime Sorcerer" },
                    { n: "Toji Fushiguro (伏黑甚尔)", s: "Sorcerer Killer" },
                    { n: "Levi Ackerman (利威尔)", s: "Humanity's Strongest" },
                    { n: "Eren Yeager (艾伦)", s: "Freedom Seeker" },
                    { n: "Zoro (索隆)", s: "Pirate Hunter" },
                    { n: "Kakashi Hatake (卡卡西)", s: "Copy Ninja" },
                    { n: "Bakugo Katsuki (爆豪胜己)", s: "Explosion Murder God" },
                    { n: "Denji (电次 - 电锯人)", s: "Chainsaw Man" },
                    { n: "Makima (玛奇玛)", s: "Control Devil" },
                    // 游戏/米哈游等
                    { n: "Zhongli (钟离 - 原神)", s: "Rex Lapis" },
                    { n: "Childe / Tartaglia (达达利亚)", s: "Harbinger" },
                    { n: "Wanderer / Scaramouche (散兵)", s: "The Balladeer" },
                    { n: "Kafka (卡芙卡 - 星铁)", s: "Stellaron Hunter" },
                    { n: "Aventurine (砂金 - 星铁)", s: "Interastral Peace" },
                    { n: "Jing Yuan (景元)", s: "Divine Foresight" },
                    { n: "Blade (刃)", s: "The Unreachable Side" },
                    { n: "Jinx (金克丝 - 英雄联盟)", s: "Loose Cannon" },
                    { n: "Ahri (阿狸)", s: "Nine-Tailed Fox" },
                    { n: "2B (尼尔机械纪元)", s: "Android Assassin" },
                    { n: "Cloud Strife (克劳德)", s: "Ex-SOLDIER" },
                    { n: "Sephiroth (萨菲罗斯)", s: "One-Winged Angel" },
                    // 乙女/韩漫/同人
                    { n: "707 (Mystic Messenger)", s: "Hacker God" },
                    { n: "Jumin Han (韩主旻)", s: "Corporate Heir" },
                    { n: "Howl Pendragon (哈尔)", s: "Wizard of Waste" },
                    { n: "Sebastian (赛巴斯蒂安)", s: "Demon Butler" },
                    { n: "Sung Jin-Woo (成振宇)", s: "Shadow Monarch" },
                    // 独立游戏/欧美动漫
                    { n: "Alastor (阿拉斯托 - 地狱客栈)", s: "Radio Demon" },
                    { n: "Angel Dust (安吉尔)", s: "Spider Demon" },
                    { n: "Lucifer Morningstar (地狱客栈)", s: "Short King Dad" },
                    { n: "Pomni (惊奇数位马戏团)", s: "Existential Jester" },
                    { n: "Sans (Undertale)", s: "Punny Skeleton" },
                    { n: "Freddy Fazbear (FNAF)", s: "Animatronic Bear" },
                    { n: "Sun/Moon (FNAF:SB)", s: "Daycare Attendant" },
                    { n: "CatNap (Poppy Playtime)", s: "Nightmare Smiling Critter" },
                    { n: "Dio Brando (迪奥)", s: "Vampire Lord" }
                ],
                DRAMA: [
                    { n: "The Detective (资深侦探)", s: "Investigation" },
                    { n: "The Commander (铁血指挥官)", s: "Military" },
                    { n: "The Diplomat (温和外交官)", s: "Politics" },
                    { n: "The Double Agent (双面间谍)", s: "Espionage" },
                    { n: "The Fallen Monarch (废黜君主)", s: "Kingdom" },
                    { n: "Survivalist (末日幸存者)", s: "Post-Apocalypse" },
                    { n: "Cybernetic Merc (赛博雇佣兵)", s: "Sci-Fi" },
                    { n: "Eldritch Scholar (禁忌学者)", s: "Lovecraftian" },
                    { n: "Wild West Outlaw (西部亡命徒)", s: "Cowboy" },
                    { n: "Vampire Noble (吸血鬼贵族)", s: "Dark Fantasy" }
                ],
                ROMANCE: [
                    { n: "Obsessive Boss (偏执总裁)", s: "Modern AU" },
                    { n: "Broken Soldier (破碎的士兵)", s: "Military AU" },
                    { n: "Yandere Stalker (病娇跟踪者)", s: "Dark Romance" },
                    { n: "Mafia Don (黑帮教父)", s: "Crime World" },
                    { n: "Forbidden Professor (禁欲系教授)", s: "Campus AU" },
                    { n: "Grumpy Rival (暴躁死对头)", s: "Enemies to Lovers" },
                    { n: "Soft Golden Retriever (金毛犬系男友)", s: "Sunshine x Sunshine" },
                    { n: "Strict Guardian (严厉监护人)", s: "Age Gap" },
                    { n: "Injured Assassin (受伤的刺客)", s: "Hurt/Comfort" },
                    { n: "Cursed Duke (受诅咒的公爵)", s: "Historical" }
                ],
                FUNC: [
                    { n: "Criminal Lawyer (刑事律师)", s: "Legal Aid" },
                    { n: "Trauma Therapist (创伤治疗师)", s: "Counseling" },
                    { n: "Elite PT (顶奢私教)", s: "Fitness" },
                    { n: "Medical Consultant (医疗顾问)", s: "Health" },
                    { n: "Fashion Stylist (造型顾问)", s: "Fashion" },
                    { n: "Language Tutor (语言私教)", s: "Academic" },
                    { n: "Career Mentor (职场导师)", s: "Professional" },
                    { n: "Culinary Expert (烹饪专家)", s: "Lifestyle" }
                ],
                MEME: [
                    { n: "Sassy AI (毒舌AI助手动)", s: "Roast Master" },
                    { n: "Disappointed Cat (失望真猫)", s: "Judgemental" },
                    { n: "Emotional Rock (情感支柱石)", s: "Silent" },
                    { n: "Untitled Goose (那只鹅)", s: "Chaos" },
                    { n: "Yandere Toaster (病娇吐司机)", s: "Cursed Object" },
                    { n: "The Karen (凯伦大妈)", s: "Complaint" },
                    { n: "Zen Capybara (佛系水豚)", s: "Chill" },
                    { n: "Talking Slime (说话史莱姆)", s: "Isekai Meme" },
                    { n: "Biblical Angel (圣经描述天使)", s: "Terrifying" }
                ]
            },
            personality: {
                IP_REAL: ["#Possessive (占有欲强)", "#Stoic (冷静禁欲)", "#Protective (护短)", "#Flirty (迷人爱撩)", "#RedFlag (危险红旗)", "#Broken (破碎忧郁)", "#Alpha (绝对强势)", "#MorallyGrey (道德灰色)", "#Sarcastic (毒舌讽刺)", "#Jealous (容易吃醋)", "#Dominant (强势掌控)", "#Grumpy (脾气暴躁)", "#Mysterious (神秘莫测)", "#GoldenRetriever (热情大狗)"],
                IP_ANIME: ["#Tsundere (傲娇)", "#Yandere (病娇)", "#Kuudere (高冷无口)", "#Deredere (开朗元气)", "#Himedere (公主病)", "#AraAra (大姐姐)", "#Dandere (害羞社恐)", "#Chuunibyou (中二病)", "#Furry (福瑞控)", "#Sadistic (抖S倾向)", "#Cunning (狡黠腹黑)", "#Edgy (阴郁酷哥)"],
                DRAMA: ["#Rational (极端理性)", "#Strategic (睿智果决)", "#Cynical (愤世嫉俗)", "#Ruthless (冷酷无情)", "#Vengeful (一心复仇)", "#Diplomatic (老练稳重)", "#Honorable (正直重荣誉)", "#Observant (洞察入微)"],
                ROMANCE: ["#Obsessive (病态痴迷)", "#Gentle (极致温柔)", "#Toxic (毒性控制)", "#Submissive (顺从温和)", "#Sassy (作精娇气)", "#Protective (保护欲过剩)", "#Angsty (虐心纠结)", "#Clingy (粘人精)"],
                FUNC: ["#Professional (极度专业)", "#Sharp (眼光犀利)", "#Patient (极度耐心)", "#Encouraging (励志暖心)", "#Strict (极其严苛)", "#Judgemental (批判性强)", "#Efficient (高效干练)"],
                MEME: ["#Chaotic (纯粹混乱)", "#Sarcastic (阴阳怪气)", "#Judgemental (眼神蔑视)", "#Chill (极致佛系)", "#Cringe (尴尬卖萌)", "#Terrifying (神性恐怖)", "#Obnoxious (令人讨厌)"]
            },
            plot: {
                DRAMA: [
                    ["#Political Intrigue (权谋暗斗)", "#Strategy (大局掌控)", "#Succession (王位争夺)"],
                    ["#Murder Mystery (悬疑解谜)", "#Heist (精密劫案)", "#Betrayal (反水背叛)"],
                    ["#Survival (求生挑战)", "#Warfare (现代战争)", "#Infiltration (潜入调查)"]
                ],
                ROMANCE: [
                    ["#Enemies to Lovers (死对头变情人)", "#Fake Dating (假扮情侣)", "#Slow Burn (慢热拉扯)"],
                    ["#Office Romance (职场潜规则)", "#Mafia Ties (黑帮羁绊)", "#Forbidden (禁忌之恋)"],
                    ["#Rainy Night (雨夜互助)", "#In The Car (密闭车内)", "#Post-Workout (健身后张力)"],
                    ["#Age Gap (大叔文学)", "#Grumpy x Sunshine (暴躁x阳光)", "#Who Did This (谁伤了你)"]
                ],
                FUNC: [
                    ["#Legal Analysis (法务分析)", "#Mock Trial (模拟法庭)", "#Advice (法律建议)"],
                    ["#Intensive Training (地狱训练)", "#Meal Plan (饮食监督)", "#Physique (身材监测)"],
                    ["#Therapy (心理疏导)", "#Healing (创伤治愈)", "#SafeSpace (避风港)"],
                    ["#Style Rating (穿搭评分)", "#GlowUp (形象改造)", "#Visual (审美指导)"]
                ],
                MEME: [
                    ["#TruthBomb (真相痛击)", "#Roast (疯狂吐槽)", "#Cringe (极致尴尬)"],
                    ["#Chaos (制造混乱)", "#Annoying (烦人攻击)", "#Silence (无声审判)"],
                    ["#UwU Arrest (猛男卖萌逮捕)", "#Honk (大白鹅尖叫)", "#Glitch (崩坏时刻)"]
                ]
            },
            scenario: {
                IP_REAL: ["#Cyberpunk (赛博朋克)", "#High Fantasy (中土世界)", "#Dark Fantasy (暗黑权游风)", "#Victorian (维多利亚时代)", "#WildWest (荒野西部)", "#Regency (摄政时期)", "#ModernWarfare (现代战场)"],
                IP_ANIME: ["#Isekai (异世界)", "#MagicAcademy (魔法学园)", "#DemonRealm (魔界)", "#Steampunk (蒸汽朋克)", "#Omegaverse (ABO设定)", "#Mecha (机甲未来)", "#YokaiVillage (妖怪村)"],
                GENERAL: ["Neon Alley (霓虹巷)", "Private Jet (私教飞机)", "interrogation Room (审讯室)", "Grand Library (大图书馆)", "Neon Hospital (科幻医院)", "Abandoned Asylum (废弃疯人院)", "Luxury Penthouse (顶层豪宅)"]
            }
        };

        function setCategory(cat, el) {
            curCat = cat;
            document.querySelectorAll('.category-btn').forEach(b => b.classList.remove('active'));
            el.classList.add('active');
            document.getElementById('deck-view').classList.remove('hidden');
            rollAll();
        }

        function roll(type) {
            const el = document.getElementById(`s-${type.slice(0,4)}`);
            if (type === 'character') {
                const pool = DB.character[curCat] || DB.character.IP_REAL;
                const r = pool[Math.floor(Math.random() * pool.length)];
                document.getElementById('s-char').innerText = r.n;
                document.getElementById('s-char-sub').innerText = `CATEGORY: ${r.s}`;
            } else if (type === 'personality') {
                const pool = DB.personality[curCat] || DB.personality.IP_REAL;
                const selected = pool.sort(() => 0.5 - Math.random()).slice(0, 3);
                el.innerHTML = selected.map(t => `<span class="tag-badge text-purple-400">${t}</span>`).join('');
            } else if (type === 'plot') {
                let pool = DB.plot[curCat] || DB.plot.ROMANCE;
                if(curCat.includes('IP')) pool = DB.plot.ROMANCE;
                const set = pool[Math.floor(Math.random() * pool.length)];
                el.innerHTML = set.map(t => `<span class="tag-badge text-rose-400">${t}</span>`).join('');
            } else if (type === 'scenario') {
                const pool = DB.scenario[curCat] || DB.scenario.GENERAL;
                const selected = pool.sort(() => 0.5 - Math.random()).slice(0, 2);
                el.innerHTML = selected.map(t => `<span class="tag-badge text-amber-400">${t}</span>`).join('');
            }
            
            const card = el.closest('.glass-card');
            card.style.animation = 'none'; void card.offsetWidth; card.style.animation = 'popIn 0.4s ease-out';
        }

        function rollAll() {
            roll('character');
            roll('personality');
            roll('plot');
            roll('scenario');
        }

        function copyIdea() {
            const c = document.getElementById('s-char').innerText;
            const cs = document.getElementById('s-char-sub').innerText;
            const p = Array.from(document.querySelectorAll('#s-pers span')).map(s => s.innerText).join(', ');
            const l = Array.from(document.querySelectorAll('#s-plot span')).map(s => s.innerText).join(', ');
            const s = Array.from(document.querySelectorAll('#s-scen span')).map(s => s.innerText).join(' / ');

            const text = `【AIRP MASTER 灵感导出】\n角色：${c}\n子类：${cs}\n性格：${p}\n情节：${l}\n场景：${s}`;
            
            const ta = document.createElement('textarea');
            ta.value = text; document.body.appendChild(ta);
            ta.select(); document.execCommand('copy'); document.body.removeChild(ta);
            
            const b = event.target;
            const old = b.innerText;
            b.innerText = "COPIED! 复制成功";
            b.style.background = "#059669";
            setTimeout(() => { b.innerText = old; b.style.background = "white"; }, 1500);
        }
    </script>
</body>
</html>
