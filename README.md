<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>台灣籃球生涯：終極完全體</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
        body { background-color: #0b0e14; color: #e2e8f0; display: flex; justify-content: center; min-height: 100vh; padding: 12px; }
        .game-container { width: 100%; max-width: 440px; background: #131924; border: 1px solid #2a3447; border-radius: 16px; padding: 16px; display: flex; flex-direction: column; gap: 10px; box-shadow: 0 10px 30px rgba(0,0,0,0.8); }
        .header { text-align: center; border-bottom: 2px solid #26334d; padding-bottom: 8px; }
        .header h1 { font-size: 17px; color: #ff6b00; letter-spacing: 0.5px; }
        .badge-bar { display: flex; justify-content: space-between; align-items: center; background: #1a2333; padding: 6px 10px; border-radius: 8px; font-size: 11px; }
        .badge { background: #ff6b00; color: #000; font-weight: bold; padding: 2px 6px; border-radius: 4px; }
        .stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 6px; background: #18202e; padding: 8px; border-radius: 10px; font-size: 11px; border: 1px solid #26334d; }
        .stat-item span { color: #38bdf8; font-weight: bold; float: right; }
        .log-box { background: #080c14; border: 1px solid #1e293b; border-radius: 10px; height: 160px; padding: 8px; overflow-y: auto; font-size: 11px; line-height: 1.5; color: #cbd5e1; }
        .log-entry { margin-bottom: 4px; border-bottom: 1px dashed #1e293b; padding-bottom: 2px; }
        .action-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 6px; }
        .btn { background: #1e293b; color: #f8fafc; border: 1px solid #334155; padding: 9px; border-radius: 8px; font-weight: bold; font-size: 11px; cursor: pointer; text-align: center; }
        .btn-primary { background: #ff6b00; color: #000; border: none; font-size: 12px; grid-column: span 2; }
        .btn-warning { background: #d97706; color: #fff; border: none; }
        .btn-success { background: #16a34a; color: #fff; border: none; }
        .select-box { background: #182232; border: 1px solid #ff6b00; border-radius: 12px; padding: 10px; display: none; }
        .school-list { max-height: 240px; overflow-y: auto; display: flex; flex-direction: column; gap: 5px; }
        .school-btn { background: #0f172a; border: 1px solid #334155; padding: 7px; border-radius: 6px; color: #f8fafc; font-size: 11px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
        .end-screen { display: none; background: linear-gradient(135deg, #1e1b4b, #311042); border: 2px solid #ffd700; border-radius: 12px; padding: 14px; text-align: center; }
    </style>
</head>
<body>

<div class="game-container">
    <div class="header">
        <h1>🏀 台灣籃球生涯：終極完全體</h1>
    </div>

    <div class="badge-bar">
        <span id="stageBadge" class="badge">🎒 JHBL 國中賽季</span>
        <span id="currentYearText" style="color:#ff6b00; font-weight:bold;">國一 (預賽)</span>
    </div>

    <div class="stats-grid">
        <div class="stat-item">球隊：<span id="pTeam">未選擇</span></div>
        <div class="stat-item">心態/士氣：<span id="pMorale" style="color:#facc15">平穩 (100)</span></div>
        <div class="stat-item">能力(OVR)：<span id="pOvr">52</span></div>
        <div class="stat-item">精力：<span id="pEnergy">100%</span></div>
        <div class="stat-item">健康狀況：<span id="pHealth" style="color:#4ade80">健康</span></div>
        <div class="stat-item">身高/體重：<span id="pBody">168cm/55kg</span></div>
    </div>

    <!-- 選擇區 -->
    <div id="selectBox" class="select-box">
        <div id="selectTitle" style="font-size:12px; font-weight:bold; color:#ff6b00; margin-bottom:6px; text-align:center;"></div>
        <div id="selectList" class="school-list"></div>
    </div>

    <div id="msgBox" style="color:#f87171; font-size:11px; text-align:center; min-height:14px; font-weight:bold;"></div>

    <div id="logBox" class="log-box">
        <div class="log-entry">> 歡迎來到《台灣籃球生涯模擬器：終極版》！請選擇你的 JHBL 開局學校：</div>
    </div>

    <!-- 操作按鈕區 -->
    <div id="controlPanel" class="action-grid" style="display:none;">
        <button class="btn btn-primary" id="btnMatch" onclick="playMatch()">🏟️ 出戰賽事 (資格/預賽/複賽/決賽)</button>
        <button class="btn" onclick="triggerStoryEvent()">📖 觸發校園與隊友劇情</button>
        <button class="btn btn-warning" onclick="openTrainMenu()">🏋️ 自主特訓項目</button>
        <button class="btn btn-success" style="grid-column: span 2;" onclick="rest()">💤 休息恢復與理療 (精力 +45)</button>
    </div>

    <!-- 生涯結算 -->
    <div id="endScreen" class="end-screen">
        <div style="font-size:16px; color:#ffd700; font-weight:bold; margin-bottom:6px;">🏆 職籃選秀與生涯最終結算</div>
        <div id="draftResultText" style="color:#38bdf8; font-weight:bold; font-size:13px; margin-bottom:8px;"></div>
        <div id="endStatsBody" style="font-size:11px; color:#e2e8f0; line-height:1.6; text-align:left; background:rgba(0,0,0,0.4); padding:8px; border-radius:6px; margin-bottom:8px;"></div>
        <button class="btn btn-primary" onclick="location.reload()">🔄 重開新角色</button>
    </div>
</div>

<script>
    let yearIdx = 0, stageIdx = 0, isHBL = false, school = "";
    let ovr = 52, energy = 100, height = 168, weight = 55, morale = 100;
    let injuryTurns = 0, totalPts = 0;

    const stages = ["資格賽", "預賽", "12強複賽", "8強準決賽", "總決賽"];
    const yearNames = ["國一", "國二", "國三", "高一", "高二", "高三"];

    // 整合所有提及的 JHBL 學校
    const jhblSchools = [
        { name: "金華國中 (台北)", bonus: 8, desc: "【北部豪門】旅外搖籃，全場壓迫戰術" },
        { name: "明仁國中 (苗栗)", bonus: 8, desc: "【中部鋼鐵】強悍身體對抗，鐵血訓練" },
        { name: "安康國中 (新北)", bonus: 7, desc: "【北部霸主】鐵血防守，體能怪物陣容" },
        { name: "中和國中 (新北)", bonus: 6, desc: "【新北勁旅】堅韌防守，陣容對抗力強" },
        { name: "員東國中 (彰化)", bonus: 6, desc: "【彰化強勢】球風強悍，外線火力猛烈" },
        { name: "大成國中 (桃園)", bonus: 6, desc: "【桃園強隊】跑轟戰術，外線火力" },
        { name: "信義國中 (台北)", bonus: 6, desc: "【北市勁旅】團隊默契，體系戰術流暢" },
        { name: "崇倫國中 (台中)", bonus: 6, desc: "【中部黑馬】快速攻防轉換，球風剽悍" },
        { name: "神岡國中 (台中)", bonus: 6, desc: "【台中強權】禁區高度優勢，強攻內線" },
        { name: "溪湖國中 (彰化)", bonus: 6, desc: "【彰化之光】頑強防守，意志力堅定" },
        { name: "太子國中 (台南)", bonus: 6, desc: "【南部勁旅】府城籃球搖籃，球風靈活" },
        { name: "七賢國中 (高雄)", bonus: 6, desc: "【港都老牌】傳統名校，基本功紮實" },
        { name: "自強國中 (花蓮)", bonus: 5, desc: "【東部勁旅】天生體能極佳，全場飛奔" },
        { name: "復興國中 (宜蘭)", bonus: 5, desc: "【宜蘭強隊】拼勁十足，快攻犀利" },
        { name: "玉里國中 (花蓮)", bonus: 5, desc: "【後山黑馬】防守強悍，韌性極佳" },
        { name: "永和國中 (新北)", bonus: 5, desc: "【新北勁旅】戰術執行力強" },
        { name: "埔里國中 (南投)", bonus: 5, desc: "【南投強權】山城熱血球風" },
        { name: "金城國中 (金門)", bonus: 5, desc: "【戰地勇士】離島熱血代表" },
        { name: "員林國中 (彰化)", bonus: 4, desc: "【彰化新銳】區域勁旅" },
        { name: "大雅國中 (台中)", bonus: 4, desc: "【中部黑馬】快速切傳打法" },
        { name: "光榮國中 (新北)", bonus: 4, desc: "【新北傳統】注重團隊配合" },
        { name: "地區乙級社團國中", bonus: 3, desc: "【乙級傳奇】無限開火權，從零開始" }
    ];

    // 整合所有 35 所 HBL 高中學校名單
    const hblSchools = [
        { name: "松山高中", req: 70, bonus: 13, desc: "【台北】綠色神盾 | 防守地獄 (門檻 OVR 70)" },
        { name: "南山高中", req: 70, bonus: 13, desc: "【新北】傳統豪門 | 天龍陣容 (門檻 OVR 70)" },
        { name: "光復高中", req: 68, bonus: 12, desc: "【新竹】籃猿霸主 | 強悍天際線 (門檻 OVR 68)" },
        { name: "能仁家商", req: 67, bonus: 11, desc: "【新北】角雕軍團 | 快速跑轟 (門檻 OVR 67)" },
        { name: "泰山高中", req: 66, bonus: 11, desc: "【新北】黑色颶風 | 快速反擊 (門檻 OVR 66)" },
        { name: "東泰高中", req: 65, bonus: 10, desc: "【新竹】竹縣勁旅 | 美式打法 (門檻 OVR 65)" },
        { name: "高苑工商", req: 64, bonus: 10, desc: "【高雄】綠魔鬼 | 南部霸主 (門檻 OVR 64)" },
        { name: "東山高中", req: 63, bonus: 9, desc: "【台中】戰鬥鴨 | 中部防守強權 (門檻 OVR 63)" },
        { name: "南湖高中", req: 63, bonus: 9, desc: "【台北】南湖風暴 | 強烈外線火力 (門檻 OVR 63)" },
        { name: "錦和高中", req: 62, bonus: 8, desc: "【新北】新銳黑馬 | 頑強球風 (門檻 OVR 62)" },
        { name: "三重商工", req: 62, bonus: 8, desc: "【新北】橘色旋風 | 團隊體系 (門檻 OVR 62)" },
        { name: "治平高中", req: 60, bonus: 7, desc: "【桃園】桃園勁旅 | 快速攻防 (門檻 OVR 60)" },
        { name: "宜蘭高中", req: 60, bonus: 7, desc: "【宜蘭】噶瑪蘭勇士 | 宜蘭之光 (門檻 OVR 60)" },
        { name: "青年高中", req: 60, bonus: 7, desc: "【台中】中部勁旅 | 激情打法 (門檻 OVR 60)" },
        { name: "三民家商", req: 60, bonus: 7, desc: "【高雄】三民家商 | 137體系 (門檻 OVR 60)" },
        { name: "屏東高中", req: 60, bonus: 7, desc: "【屏東】南霸天 | 熱血魂 (門檻 OVR 60)" },
        { name: "北市成功高中", req: 60, bonus: 7, desc: "【台北】學術與籃球兼備 (門檻 OVR 60)" },
        { name: "同德高中", bonus: 6, desc: "【南投】南投甲級勁旅 (無門檻)" },
        { name: "民生家商", bonus: 6, desc: "【屏東】屏東甲級新勢力 (無門檻)" },
        { name: "花蓮體中", bonus: 6, desc: "【花蓮】東部體育搖籃 (無門檻)" },
        { name: "馬祖高中", bonus: 6, desc: "【連江】離島熱血代表 (無門檻)" },
        { name: "臺東高中", bonus: 6, desc: "【台東】後山籃球勇士 (無門檻)" },
        { name: "開南高中", bonus: 6, desc: "【台北】北市甲級新戰力 (無門檻)" },
        { name: "中信高中", bonus: 6, desc: "【台南】台南興起勁旅 (無門檻)" },
        { name: "后綜高中", bonus: 6, desc: "【台中】后綜奇蹟信念 (無門檻)" },
        { name: "永平工商", bonus: 6, desc: "【桃園】桃園甲級代表 (無門檻)" },
        { name: "彰縣成功高中", bonus: 5, desc: "【彰化】彰化在地鋼鐵軍 (無門檻)" },
        { name: "新民高中", bonus: 5, desc: "【台中】台中甲級戰隊 (無門檻)" },
        { name: "和平高中", bonus: 5, desc: "【台北】北市甲級強攻隊 (無門檻)" },
        { name: "東吳工家", bonus: 5, desc: "【嘉義】雲嘉南甲級代表 (無門檻)" },
        { name: "北科附工", bonus: 5, desc: "【桃園】桃園傳統強隊 (無門檻)" },
        { name: "木柵高工", bonus: 5, desc: "【台北】木柵黑馬 (無門檻)" },
        { name: "海星高中", bonus: 5, desc: "【花蓮】花蓮後起之秀 (無門檻)" },
        { name: "新屋高中", bonus: 5, desc: "【桃園】桃園新銳力量 (無門檻)" },
        { name: "滬江高中", bonus: 5, desc: "【台北】北市強悍勁旅 (無門檻)" }
    ];

    const storyEvents = [
        {
            title: "🚨 隊內練習賽與學長爆發激烈口角！",
            options: [
                { text: "正面對決！在單挑中連下三分打服學長", effect: () => { ovr += 2; morale -= 10; return "你在單挑中完勝！能力 +2，但球隊氣氛略顯尷尬。"; } },
                { text: "主動遞水道歉，展現謙虛球風", effect: () => { morale += 15; return "學長傳授私房切入技巧！心態 +15。"; } }
            ]
        },
        {
            title: "📺 知名籃球媒體來訪問你！",
            options: [
                { text: "放話：「目標就是帶隊衝進台北小巨蛋決賽！」", effect: () => { morale += 20; return "媒體大肆報導，士氣大升！"; } },
                { text: "低調回應：「一步一腳印，執行教練指示。」", effect: () => { ovr += 1; return "教練對你的沉穩非常滿意，給予更多單打權！"; } }
            ]
        }
    ];

    function initGame() {
        showSelect(jhblSchools, "🎒 請選擇你的 JHBL 開局國中學校：", (s) => {
            school = s.name; ovr += s.bonus;
            addLog(`加盟<b>【${school}】</b>！正式展開國中生涯！`);
            document.getElementById('selectBox').style.display = 'none';
            document.getElementById('controlPanel').style.display = 'grid';
            updateUI();
        });
    }

    function showSelect(list, title, callback) {
        document.getElementById('selectBox').style.display = 'block';
        document.getElementById('selectTitle').innerText = title;
        let el = document.getElementById('selectList');
        el.innerHTML = "";
        list.forEach(item => {
            let btn = document.createElement('div');
            btn.className = 'school-btn';
            let isLocked = (item.req && ovr < item.req);
            if (isLocked) {
                btn.style.opacity = '0.4';
                btn.innerHTML = `<div><b>${item.name}</b><br><small style="color:#f87171">${item.desc}</small></div>`;
            } else {
                btn.onclick = () => callback(item);
                btn.innerHTML = `<div><b>${item.name}</b><br><small style="color:#94a3b8">${item.desc}</small></div>`;
            }
            el.appendChild(btn);
        });
    }

    function updateUI() {
        document.getElementById('pTeam').innerText = school || "未選";
        document.getElementById('pMorale').innerText = `${morale}`;
        document.getElementById('currentYearText').innerText = `${yearNames[yearIdx]} (${stages[stageIdx]})`;
        document.getElementById('pOvr').innerText = ovr;
        document.getElementById('pEnergy').innerText = energy + "%";
        document.getElementById('pBody').innerText = `${height}cm/${weight}kg`;

        let hEl = document.getElementById('pHealth');
        if (injuryTurns > 0) {
            hEl.innerText = `傷兵(休養 ${injuryTurns} 場)`;
            hEl.style.color = "#f87171";
            document.getElementById('btnMatch').disabled = true;
        } else {
            hEl.innerText = "健康";
            hEl.style.color = "#4ade80";
            document.getElementById('btnMatch').disabled = false;
        }

        if (isHBL) {
            document.getElementById('stageBadge').innerText = "🔥 HBL 高中聯賽";
            document.getElementById('stageBadge').style.background = "#e11d48";
        }
    }

    function addLog(m) {
        let b = document.getElementById('logBox');
        b.innerHTML += `<div class="log-entry">> ${m}</div>`;
        b.scrollTop = b.scrollHeight;
    }

    function setMsg(m) { document.getElementById('msgBox').innerText = m; }

    function playMatch() {
        setMsg("");
        if (energy < 25) return setMsg("⚠️ 體力過低！請進行休息。");
        energy -= 25;

        if (Math.random() < 0.08 && injuryTurns === 0) {
            injuryTurns = Math.floor(Math.random() * 2) + 1;
            addLog(`<span style="color:#f87171">💥 比賽中落地扭傷！需休養 ${injuryTurns} 場賽事！</span>`);
            updateUI();
            return;
        }

        let pts = Math.floor(ovr / 3 + Math.random() * 12);
        totalPts += pts;
        addLog(`🏟️ 【${stages[stageIdx]}】結束！你轟下 <b style="color:#38bdf8">${pts}</b> 分！`);

        stageIdx++;
        if (stageIdx >= stages.length) {
            stageIdx = 0;
            yearIdx++;
            addLog(`<b>🎉 賽季結束！進入 ${yearNames[yearIdx] || "畢業"} 階段！</b>`);
        }

        if (yearIdx === 3 && !isHBL) {
            isHBL = true;
            addLog("<b>🎓 JHBL 國中畢業！進入 35 所 HBL 高中招生招募！</b>");
            showSelect(hblSchools, "🔥 請選擇你要加盟的 HBL 高中：", (s) => {
                school = s.name; ovr += s.bonus;
                addLog(`加盟<b>【${school}】</b>！踏上 HBL 征途！`);
                document.getElementById('selectBox').style.display = 'none';
                updateUI();
            });
        } else if (yearIdx >= 6) {
            startProfessionalDraft();
        }
        updateUI();
    }

    function triggerStoryEvent() {
        setMsg("");
        let ev = storyEvents[Math.floor(Math.random() * storyEvents.length)];
        let options = ev.options.map(o => ({ name: o.text, desc: "", action: o.effect }));
        showSelect(options, ev.title, (opt) => {
            let res = opt.action();
            addLog(`<span style="color:#facc15">📖 劇情結果：${res}</span>`);
            document.getElementById('selectBox').style.display = 'none';
            updateUI();
        });
    }

    function openTrainMenu() {
        setMsg("");
        let menus = [
            { name: "🎯 投籃與外線特訓", desc: "OVR +2，精力 -20", act: () => { ovr += 2; energy -= 20; } },
            { name: "🏋️ 力量與重訓對抗", desc: "OVR +1、體重 +1kg，精力 -20", act: () => { ovr += 1; weight += 1; energy -= 20; } }
        ];
        showSelect(menus, "🏋️ 選擇自主特訓項目：", (m) => {
            if (energy < 20) return setMsg("⚠️ 體力不足！");
            m.act();
            addLog(`完成訓練！能力提升至 <b>${ovr}</b>！`);
            document.getElementById('selectBox').style.display = 'none';
            updateUI();
        });
    }

    function rest() {
        setMsg("");
        energy = Math.min(100, energy + 45);
        if (injuryTurns > 0) {
            injuryTurns--;
            addLog(`💤 冰療復健中，剩餘傷停：${injuryTurns} 場。`);
        } else {
            if (Math.random() < 0.30 && height < 205) {
                height += 1;
                addLog(`💤 充分休息，身高抽高 <b>+1cm</b> (${height}cm)！`);
            } else {
                addLog("💤 充足睡眠，體力恢復！");
            }
        }
        updateUI();
    }

    function startProfessionalDraft() {
        document.getElementById('controlPanel').style.display = 'none';
        let draftText = "";
        if (ovr >= 85) draftText = "👑 TPBL / PLG 職籃選秀【狀元指名】";
        else if (ovr >= 76) draftText = "🌟 TPBL / PLG 職籃選秀【首輪前三順位】";
        else if (ovr >= 68) draftText = "🏀 SBL 職籃選秀【首輪指名】";
        else draftText = "💪 自由球員試訓 / 業餘 MVP";

        document.getElementById('draftResultText').innerText = draftText;
        document.getElementById('endStatsBody').innerHTML = 
            `🎓 高中加盟學校：<b>${school}</b><br>` +
            `📏 最終體格：<b>${height} cm / ${weight} kg</b><br>` +
            `🔥 綜合能力 (OVR)：<b>${ovr}</b><br>` +
            `🏀 生涯總得分：<b>${totalPts} 分</b>`;

        document.getElementById('endScreen').style.display = 'block';
        addLog(`<span style="color:#ffd700">🏆 你的台灣學生籃球生涯正式劃下句點！成功進入職籃！</span>`);
    }

    initGame();
</script>
</body>
</html>
