# alick 毕业快乐趣味交互网站 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建一个在手机端流畅运行的、包含树洞相识、王者连击游戏、手势拨穗、扔学士帽及照片展示等互动环节的可爱俏皮毕业祝贺单页网站。

**Architecture:** 单个 HTML 文件，包含核心 DOM、嵌入式 Vanilla CSS 样式和交互 JavaScript 逻辑，引用 canvas-confetti CDN 实现粒子动效，并从本地 assets/ 目录加载 8 张照片。

**Tech Stack:** HTML5, CSS3, ES6 JavaScript, canvas-confetti (via CDN)

## Global Constraints
- 必须兼容 iOS/Android 手机微信内置浏览器。
- 严禁引入 heavy/臃肿的框架，采用单 HTML 文件。
- 页面必须支持 prefers-reduced-motion 的 CSS 动画减弱处理。
- 所有照片的路径对应为 assets/ 目录下的 `photo_0.jpg` 到 `photo_7.jpg`。

---

### Task 1: 目录初始化与素材拷贝

**Files:**
- Create: `assets/`
- Modify: None
- Test: None

**Interfaces:**
- Produces: 8张照片保存在 `assets/` 目录下

- [ ] **Step 1: 创建 assets 目录**
  创建 `/Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/` 文件夹。

- [ ] **Step 2: 拷贝 8 张照片**
  执行命令将用户上传的图片拷贝并重命名至 `assets/`：
  ```bash
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990111833.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_0.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990138766.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_1.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990163771.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_2.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990171279.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_3.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990188093.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_4.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990256612.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_5.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990268874.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_6.jpg
  cp /Users/zzzhl/.gemini/antigravity/brain/05c5da61-b503-4122-9d7a-b9fdd51d0d36/media__1782990274644.jpg /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/photo_7.jpg
  ```

- [ ] **Step 3: 检查拷贝结果**
  运行：`ls -l /Users/zzzhl/.gemini/antigravity/scratch/alick-grad/assets/`
  预期：输出中有且仅有 photo_0.jpg 到 photo_7.jpg 且大小正常。

- [ ] **Step 4: 提交**
  ```bash
  git add assets/
  git commit -m "chore: copy photo assets to project"
  ```

---

### Task 2: HTML/CSS 骨架与基础样式系统

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: 响应式页面结构、CSS 变量、背景气泡粒子动画、音轨组件接口。

- [ ] **Step 1: 编写基础 index.html 框架和 Vanilla CSS 样式系统**
  包括柔和毛玻璃样式（Glassmorphism）、背景漂浮爱心气泡动画、音轨开关按钮及通用转场控制。
  ```html
  <!DOCTYPE html>
  <html lang="zh-CN">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>alick 毕业快乐！🎓</title>
    <style>
      :root {
        --pku-red: #9C2C2C;
        --love-pink: #FFF5F5;
        --gold: #D4AF37;
        --text-dark: #333333;
      }
      * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
      body, html {
        width: 100%;
        height: 100%;
        overflow: hidden;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        background: var(--love-pink);
        color: var(--text-dark);
      }
      .bg-bubbles {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; pointer-events: none; overflow: hidden;
      }
      .bubble {
        position: absolute; bottom: -50px; background: rgba(255, 182, 193, 0.4); border-radius: 50%;
        animation: floatUp 8s infinite linear;
      }
      @keyframes floatUp {
        0% { transform: translateY(0) rotate(0deg); opacity: 0; }
        10% { opacity: 0.8; }
        90% { opacity: 0.8; }
        100% { transform: translateY(-110vh) rotate(360deg); opacity: 0; }
      }
      .screen {
        position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 2;
        display: flex; flex-direction: column; align-items: center; justify-content: center;
        opacity: 0; transform: scale(0.95); transition: opacity 0.6s, transform 0.6s, display 0.6s allow-discrete, overlay 0.6s allow-discrete;
        display: none; padding: 20px;
      }
      .screen.active {
        display: flex; opacity: 1; transform: scale(1);
        @starting-style { opacity: 0; transform: scale(0.95); }
      }
      .card {
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(12px);
        -webkit-backdrop-filter: blur(12px);
        border: 1px solid rgba(255, 255, 255, 0.4);
        border-radius: 24px;
        padding: 30px 24px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
        width: 100%; max-width: 380px; text-align: center; z-index: 3;
      }
      .music-btn {
        position: fixed; top: 20px; right: 20px; z-index: 99; width: 40px; height: 40px;
        border-radius: 50%; background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(5px);
        display: flex; align-items: center; justify-content: center; border: none; box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      }
      .music-btn.playing { animation: spin 3s infinite linear; }
      @keyframes spin { 100% { transform: rotate(360deg); } }
      .pku-btn {
        background: var(--pku-red); color: white; border: none; padding: 12px 30px;
        border-radius: 50px; font-size: 16px; font-weight: bold; cursor: pointer;
        box-shadow: 0 4px 15px rgba(156, 44, 44, 0.3); transition: transform 0.2s, box-shadow 0.2s;
        margin-top: 20px;
      }
      .pku-btn:active { transform: scale(0.95); }
    </style>
  </head>
  <body>
    <button class="music-btn" id="musicBtn">🎵</button>
    <audio id="bgm" loop src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3"></audio>
    <div class="bg-bubbles" id="bubblesContainer"></div>
    <script>
      // 动态生成背景气泡
      const container = document.getElementById('bubblesContainer');
      for (let i = 0; i < 15; i++) {
        const bubble = document.createElement('div');
        bubble.className = 'bubble';
        const size = Math.random() * 30 + 15;
        bubble.style.width = `${size}px`;
        bubble.style.height = `${size}px`;
        bubble.style.left = `${Math.random() * 100}%`;
        bubble.style.animationDelay = `${Math.random() * 8}s`;
        bubble.style.animationDuration = `${Math.random() * 5 + 6}s`;
        container.appendChild(bubble);
      }
      // 音乐播放控制器
      const bgm = document.getElementById('bgm');
      const musicBtn = document.getElementById('musicBtn');
      musicBtn.addEventListener('click', () => {
        if (bgm.paused) {
          bgm.play().then(() => {
            musicBtn.classList.add('playing');
          }).catch(err => console.log("Audio play blocked", err));
        } else {
          bgm.pause();
          musicBtn.classList.remove('playing');
        }
      });
      // 切换Screen助手
      function showScreen(screenId) {
        document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
        document.getElementById(screenId).classList.add('active');
      }
    </script>
  </body>
  </html>
  ```

- [ ] **Step 2: 在浏览器中验证基础布局**
  用 Python 启动临时服务或直接用浏览器打开，检查气泡是否向上飘动，音量按钮是否能够静音播放。

- [ ] **Step 3: 提交**
  ```bash
  git add index.html
  git commit -m "feat: init HTML/CSS skeleton and floating bubble background"
  ```

---

### Task 3: 关卡一：北大树洞奇遇开场

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: Task 2 中的基础 CSS/JS
- Produces: 树洞开场界面 DOM，Re 树洞回复打字动效，渐变转场。

- [ ] **Step 1: 添加树洞界面的 HTML 和 CSS**
  在 `<body>` 下添加 `screen-hole` 并设计树洞样式。
  ```html
  <!-- 替换 index.html 内部的 body 部分 -->
  <div class="screen active" id="screenHole">
    <div class="card treehole-card">
      <div class="hole-header">
        <span class="hole-title">PKU Helper 树洞 🌲</span>
      </div>
      <div class="hole-content">
        <div class="hole-post init-post" id="post1">
          <span class="post-num">#102400</span> <span class="post-text">(洞主男) 好想谈恋爱 🥺</span>
        </div>
        <div class="hole-post reply-post" id="post2" style="opacity: 0;">
          <span class="reply-arrow">└ [Re]</span> <span class="post-text">a女好想谈恋爱 🙋‍♀️</span>
        </div>
      </div>
      <div class="met-heart" id="metHeart" style="opacity: 0; scale: 0.5; transition: all 0.5s;">💖</div>
      <div class="hole-tip" id="holeTip" style="opacity: 0;">一切，从这里开始……</div>
      <button class="pku-btn" id="startAdventureBtn" style="opacity: 0; transition: opacity 0.5s;">开启毕业大冒险</button>
    </div>
  </div>
  ```
  添加 CSS：
  ```css
  /* 树洞卡片特有样式 */
  .treehole-card { text-align: left; position: relative; }
  .hole-header { border-bottom: 2px solid var(--pku-red); padding-bottom: 10px; margin-bottom: 15px; }
  .hole-title { font-weight: bold; color: var(--pku-red); font-size: 18px; }
  .hole-post { padding: 12px; border-radius: 12px; background: rgba(255,255,255,0.6); margin-bottom: 12px; box-shadow: inset 0 1px 3px rgba(0,0,0,0.05); }
  .post-num { font-weight: bold; color: #888; margin-right: 5px; }
  .reply-arrow { color: var(--pku-red); font-weight: bold; margin-right: 5px; }
  .met-heart { font-size: 48px; text-align: center; margin: 15px 0; }
  .hole-tip { text-align: center; font-size: 16px; font-weight: bold; margin-top: 15px; color: var(--pku-red); }
  .treehole-card .pku-btn { width: 100%; display: block; }
  ```

- [ ] **Step 2: 添加树洞动效控制 JavaScript 脚本**
  ```javascript
  // 树洞加载动效
  document.addEventListener('DOMContentLoaded', () => {
    setTimeout(() => {
      // 1.2s后显示回复
      const post2 = document.getElementById('post2');
      post2.style.opacity = '1';
      post2.style.transition = 'opacity 0.8s';
      
      setTimeout(() => {
        // 2s后显示爱心，心跳震动
        const metHeart = document.getElementById('metHeart');
        metHeart.style.opacity = '1';
        metHeart.style.transform = 'scale(1.2)';
        
        setTimeout(() => {
          // 2.8s后显示开启按钮和提示
          document.getElementById('holeTip').style.opacity = '1';
          document.getElementById('holeTip').style.transition = 'opacity 0.8s';
          document.getElementById('startAdventureBtn').style.opacity = '1';
        }, 800);
      }, 1000);
    }, 1200);
    
    document.getElementById('startAdventureBtn').addEventListener('click', () => {
      showScreen('screenGame');
      // 触发音乐自动播放 (部分手机可能需要触发点击才允许播放音频)
      const bgm = document.getElementById('bgm');
      if (bgm.paused) {
        bgm.play().then(() => {
          document.getElementById('musicBtn').classList.add('playing');
        }).catch(e => console.log("Audio auto-play blocked", e));
      }
    });
  });
  ```

- [ ] **Step 3: 验证树洞的渐次浮现效果**
  刷新页面，确认：
  - 首条帖子立即可见。
  - 1.2秒后，女生的 Re 帖子淡入。
  - 之后粉色爱心伴随缩放动画显示。
  - 最后文案和“开启毕业大冒险”按钮显示，点击切换页面。

- [ ] **Step 4: 提交**
  ```bash
  git add index.html
  git commit -m "feat: add Level 1 Met-Cute Peking University Treehole"
  ```

---

### Task 4: 关卡二：王者荣耀连击大作战小游戏

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: Task 3 中的 screen 切换
- Produces: 连击游戏页面，星光粒子爆炸动效，王者击杀称号音效提示词。

- [ ] **Step 1: 在 index.html 中加入王者游戏屏幕**
  在 `index.html` 中加入 `screenGame` 容器。
  ```html
  <div class="screen" id="screenGame">
    <div class="card game-card">
      <div class="game-title">⚔️ 毕业大作战 ⚔️</div>
      <p class="game-subtitle">狂戳水晶，帮 alick 疯狂积攒能量毕业！</p>
      
      <div class="game-stats">
        <div class="stat-box">连击: <span id="comboCount">0</span></div>
        <div class="stat-box">快乐值: <span id="scoreVal">0</span></div>
      </div>
      
      <div class="crystal-container">
        <div class="crystal-btn" id="crystalBtn">💎</div>
        <div class="tap-effect-layer" id="tapEffects"></div>
      </div>
      
      <div class="kill-banner" id="killBanner"></div>
      <div class="victory-overlay" id="victoryOverlay" style="display: none;">
        <div class="victory-badge">VICTORY</div>
        <div class="achievement-title">👑 荣耀毕业生 · 大融城特级美食家 👑</div>
        <button class="pku-btn" onclick="showScreen('screenGraduation')">前往毕业典礼</button>
      </div>
    </div>
  </div>
  ```
  添加 CSS：
  ```css
  /* 游戏界面 */
  .game-card {
    background: radial-gradient(circle at center, rgba(16, 25, 48, 0.85) 0%, rgba(8, 12, 24, 0.95) 100%);
    color: #fff; border: 1px solid rgba(0, 191, 255, 0.3); position: relative; overflow: hidden;
  }
  .game-title { font-size: 22px; font-weight: bold; color: #00bfff; text-shadow: 0 0 10px rgba(0,191,255,0.5); margin-bottom: 5px; }
  .game-subtitle { font-size: 13px; color: #a0aec0; margin-bottom: 20px; }
  .game-stats { display: flex; justify-content: space-around; margin-bottom: 20px; }
  .stat-box { background: rgba(255,255,255,0.1); padding: 8px 15px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.2); font-weight: bold; }
  .crystal-container { position: relative; width: 140px; height: 140px; margin: 0 auto 20px; }
  .crystal-btn {
    width: 120px; height: 120px; border-radius: 50%; background: radial-gradient(circle, #00bfff, #002244);
    font-size: 60px; display: flex; align-items: center; justify-content: center; margin: 10px auto;
    box-shadow: 0 0 25px #00bfff, inset 0 0 15px rgba(255,255,255,0.6);
    user-select: none; cursor: pointer; transition: transform 0.05s;
  }
  .crystal-btn:active { transform: scale(0.9); }
  .tap-effect-layer { position: absolute; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; }
  .float-num {
    position: absolute; color: #ffd700; font-weight: bold; font-size: 20px;
    animation: floatUpWord 0.8s forwards ease-out; text-shadow: 0 2px 4px rgba(0,0,0,0.5);
  }
  @keyframes floatUpWord {
    0% { transform: translate(0, 0) scale(0.8); opacity: 1; }
    100% { transform: translate(var(--dx), var(--dy)) scale(1.2); opacity: 0; }
  }
  .kill-banner {
    height: 40px; font-size: 20px; font-weight: bold; color: #ffd700;
    text-shadow: 0 0 8px rgba(255, 215, 0, 0.7); margin-top: 10px;
    animation: pulse 1s infinite alternate; text-align: center;
  }
  @keyframes pulse { 0% { transform: scale(1); } 100% { transform: scale(1.05); } }
  .victory-overlay {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.9); display: flex; flex-direction: column;
    align-items: center; justify-content: center; z-index: 10; padding: 20px;
  }
  .victory-badge {
    font-size: 50px; font-weight: 900; color: #ffd700; text-shadow: 0 0 20px #ff8c00;
    letter-spacing: 5px; animation: victoryZoom 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    margin-bottom: 20px;
  }
  @keyframes victoryZoom { 0% { transform: scale(0); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }
  .achievement-title { font-size: 16px; font-weight: bold; color: #fff; margin-bottom: 30px; text-align: center; }
  ```

- [ ] **Step 2: 编写游戏交互逻辑 JavaScript**
  ```javascript
  const crystalBtn = document.getElementById('crystalBtn');
  const comboCountSpan = document.getElementById('comboCount');
  const scoreValSpan = document.getElementById('scoreVal');
  const killBanner = document.getElementById('killBanner');
  const tapEffects = document.getElementById('tapEffects');
  const victoryOverlay = document.getElementById('victoryOverlay');
  
  let combo = 0;
  let score = 0;
  const targetCombo = 12;
  const words = ["绩点+1", "法学学士+1", "吃大融城+1", "打王者+1", "可爱度+999"];
  const killSounds = {
    1: "First Blood! (开启绩点冲刺)",
    3: "Double Kill! (PPE全部修满)",
    6: "Triple Kill! (毕业论文通过)",
    9: "Quadra Kill! (大融城横着走)",
    12: "Penta Kill! (荣耀毕业！)"
  };
  
  crystalBtn.addEventListener('pointerdown', (e) => {
    if (combo >= targetCombo) return;
    
    combo++;
    score += Math.floor(Math.random() * 50) + 50;
    comboCountSpan.innerText = combo;
    scoreValSpan.innerText = score;
    
    // 生成浮空文字
    const float = document.createElement('div');
    float.className = 'float-num';
    float.innerText = words[Math.floor(Math.random() * words.length)];
    // 随机散射弧度
    const angle = Math.random() * Math.PI * 2;
    const distance = Math.random() * 50 + 50;
    const dx = Math.cos(angle) * distance;
    const dy = Math.sin(angle) * distance;
    float.style.setProperty('--dx', `${dx}px`);
    float.style.setProperty('--dy', `${dy}px`);
    float.style.left = `50%`;
    float.style.top = `50%`;
    tapEffects.appendChild(float);
    setTimeout(() => float.remove(), 800);
    
    // 连击提示文案
    if (killSounds[combo]) {
      killBanner.innerText = killSounds[combo];
      // 触发震动 (支持的安卓手机)
      if (navigator.vibrate) navigator.vibrate(100);
    }
    
    // 达成目标
    if (combo >= targetCombo) {
      setTimeout(() => {
        victoryOverlay.style.display = 'flex';
        // 触发一次小彩花
        if (window.confetti) {
          window.confetti({ particleCount: 80, spread: 60, origin: { y: 0.6 } });
        }
      }, 800);
    }
  });
  ```

- [ ] **Step 3: 运行并验证游戏逻辑**
  在屏幕中连续快速点击水晶，测试：
  - Combo 和快乐值累加。
  - "+1" 粒子特效从中央散射出来。
  - 到达 1, 3, 6, 9, 12 击时出现相应的王者连击击杀宣告。
  - 12 击后弹出 Victory 覆盖层，点击按钮进入下一步。

- [ ] **Step 4: 提交**
  ```bash
  git add index.html
  git commit -m "feat: add Level 2 Honor of Kings graduation challenge game"
  ```

---

### Task 5: 关卡三：拨穗、扔学士帽与照片相册

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: Task 4 中的 Victory 流程，assets/ 文件夹下的 8 张照片，以及 canvas-confetti CDN 库
- Produces: 拨穗手势卡片，物理抛物线扔学士帽，炫彩喷洒，以及恋爱/毕业照片卡片轮播。

- [ ] **Step 1: 在 index.html 头引入 canvas-confetti CDN 库**
  ```html
  <!-- 在 head 中引入 -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>
  ```

- [ ] **Step 2: 添加关卡三的 HTML DOM**
  ```html
  <div class="screen" id="screenGraduation">
    <div class="card grad-card">
      <div id="tasselStage">
        <h3 style="color: var(--pku-red); margin-bottom: 15px;">🎓 拨穗礼仪 🎓</h3>
        <p style="font-size: 14px; margin-bottom: 20px; color: #666;">请指尖向左拖拽流苏，完成拨穗仪式！</p>
        
        <div class="tassel-box">
          <div class="cap-svg-container">
            <!-- 绘制扁平学士帽 -->
            <svg viewBox="0 0 100 80" width="120" height="100">
              <path d="M 50 10 L 90 30 L 50 50 L 10 30 Z" fill="#2d3748" />
              <rect x="35" y="42" width="30" height="15" fill="#2d3748" rx="2" />
            </svg>
            <div class="tassel-line" id="tasselLine"></div>
            <div class="tassel-knot" id="tasselKnot">🔴</div>
          </div>
        </div>
        <div class="tassel-status" id="tasselStatus">等待拨穗...</div>
      </div>
      
      <div id="throwStage" style="display: none;">
        <h3 style="color: var(--pku-red); margin-bottom: 15px;">🎓 毕业快乐！ 🎓</h3>
        <p style="font-size: 14px; margin-bottom: 20px; color: #666;">点击帽子，抛向空中！</p>
        
        <div class="cap-container" id="throwCapBtn">
          <div class="flying-cap" id="flyingCap">🎓</div>
        </div>
      </div>
      
      <div id="finalCard" style="display: none;">
        <!-- 照片轮播 -->
        <div class="carousel">
          <div class="carousel-inner" id="carouselInner"></div>
          <button class="carousel-ctrl prev" onclick="moveSlide(-1)">❮</button>
          <button class="carousel-ctrl next" onclick="moveSlide(1)">❯</button>
        </div>
        
        <div class="blessing-text">
          <h2>🎉 祝 alick 毕业快乐 🎉</h2>
          <p><strong>北京大学元培学院</strong></p>
          <p>政治、经济、哲学专业 (PPE)</p>
          <p>法学学士学位 ⚖️</p>
          <hr style="margin: 10px 0; border: 0; border-top: 1px dashed rgba(156,44,44,0.3);">
          <p style="font-size: 14px; color: #6b7280; font-style: italic;">“未来打王者继续五杀，吃遍大融城，并肩前行，未来可期！”</p>
        </div>
      </div>
    </div>
  </div>
  ```
  添加 CSS：
  ```css
  /* 拨穗页面 */
  .tassel-box { position: relative; width: 200px; height: 140px; margin: 0 auto 20px; background: rgba(255,255,255,0.4); border-radius: 16px; border: 1px dashed #cbd5e1; }
  .cap-svg-container { position: relative; width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; }
  .tassel-line { position: absolute; width: 2px; height: 45px; background: var(--pku-red); top: 38px; transform-origin: top center; transition: transform 0.1s; }
  .tassel-knot { position: absolute; font-size: 20px; width: 30px; height: 30px; display: flex; align-items: center; justify-content: center; cursor: pointer; top: 78px; user-select: none; }
  .tassel-status { font-weight: bold; color: var(--pku-red); }
  
  /* 抛学士帽 */
  .cap-container { width: 100px; height: 100px; margin: 30px auto; cursor: pointer; }
  .flying-cap { font-size: 80px; text-align: center; transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94), opacity 0.8s; }
  
  /* 相册轮播 */
  .carousel { position: relative; width: 100%; height: 260px; border-radius: 16px; overflow: hidden; margin-bottom: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.15); }
  .carousel-inner { display: flex; width: 800%; height: 100%; transition: transform 0.5s ease-in-out; }
  .carousel-slide { width: 12.5%; height: 100%; flex-shrink: 0; position: relative; }
  .carousel-slide img { width: 100%; height: 100%; object-fit: cover; }
  .carousel-slide .photo-caption {
    position: absolute; bottom: 0; left: 0; width: 100%; background: rgba(0,0,0,0.6); color: white;
    font-size: 12px; padding: 6px; text-align: center; backdrop-filter: blur(4px);
  }
  .carousel-ctrl { position: absolute; top: 50%; transform: translateY(-50%); background: rgba(0,0,0,0.3); border: none; color: white; font-size: 20px; width: 36px; height: 36px; border-radius: 50%; cursor: pointer; }
  .carousel-ctrl.prev { left: 10px; }
  .carousel-ctrl.next { right: 10px; }
  .blessing-text { margin-top: 15px; text-align: center; }
  .blessing-text h2 { color: var(--pku-red); font-size: 20px; margin-bottom: 8px; }
  .blessing-text p { font-size: 15px; line-height: 1.6; }
  ```

- [ ] **Step 3: 编写交互控制 JavaScript 脚本**
  包括：
  1. PointerEvents（支持鼠标拖拽与手机滑屏触控）实现流苏拖拽；
  2. 抛帽轨迹和全屏幕爆 confetti 粒子雨；
  3. 照片集数据加载与轮播控制。
  ```javascript
  // 1. 初始化流苏拖拽
  const tasselKnot = document.getElementById('tasselKnot');
  const tasselLine = document.getElementById('tasselLine');
  const tasselStatus = document.getElementById('tasselStatus');
  const tasselStage = document.getElementById('tasselStage');
  const throwStage = document.getElementById('throwStage');
  
  // 流苏位置参数
  const centerX = 100; // svg中流苏悬挂的中心点
  let startX = 0;
  let currentAngle = 40; // 初始偏右40度
  
  // 设置流苏偏角
  function setTasselAngle(deg) {
    tasselLine.style.transform = `rotate(${deg}deg)`;
    // 近似计算流苏结的位置 (流苏线长度约45px)
    const rad = deg * Math.PI / 180;
    const x = centerX + Math.sin(rad) * 45;
    const y = 38 + Math.cos(rad) * 45;
    tasselLine.style.left = `${centerX}px`;
    tasselKnot.style.left = `${x - 15}px`;
    tasselKnot.style.top = `${y - 15}px`;
  }
  
  // 初始化默认角度
  setTasselAngle(40);
  
  let isDragging = false;
  tasselKnot.addEventListener('pointerdown', (e) => {
    isDragging = true;
    startX = e.clientX;
    tasselKnot.setPointerCapture(e.pointerId);
  });
  
  tasselKnot.addEventListener('pointermove', (e) => {
    if (!isDragging) return;
    const dx = e.clientX - startX;
    // 随拖拽位移把角度从 40度 调整到 -40度
    let targetAngle = 40 + (dx / 1.5);
    if (targetAngle > 40) targetAngle = 40;
    if (targetAngle < -40) targetAngle = -40;
    setTasselAngle(targetAngle);
    
    // 拖拽成功阈值（达到左侧-35度以下）
    if (targetAngle <= -35) {
      isDragging = false;
      tasselKnot.releasePointerCapture(e.pointerId);
      setTasselAngle(-40);
      tasselStatus.innerText = "🎉 拨穗完成！";
      
      // 1s后切换到扔帽子环节
      setTimeout(() => {
        tasselStage.style.display = 'none';
        throwStage.style.display = 'block';
      }, 1000);
    }
  });
  
  tasselKnot.addEventListener('pointerup', () => {
    if (isDragging) {
      isDragging = false;
      // 弹回原样
      setTasselAngle(40);
    }
  });
  
  // 2. 扔学士帽与 Confetti
  const throwCapBtn = document.getElementById('throwCapBtn');
  const flyingCap = document.getElementById('flyingCap');
  const finalCard = document.getElementById('finalCard');
  
  throwCapBtn.addEventListener('click', () => {
    flyingCap.style.transform = 'translateY(-300px) scale(0.1) rotate(720deg)';
    flyingCap.style.opacity = '0';
    
    // 连续喷洒五彩爆破彩纸
    let duration = 3 * 1000;
    let end = Date.now() + duration;
    
    (function frame() {
      confetti({
        particleCount: 5,
        angle: 60,
        spread: 55,
        origin: { x: 0 }
      });
      confetti({
        particleCount: 5,
        angle: 120,
        spread: 55,
        origin: { x: 1 }
      });
      
      if (Date.now() < end) {
        requestAnimationFrame(frame);
      }
    }());
    
    setTimeout(() => {
      throwStage.style.display = 'none';
      finalCard.style.display = 'block';
      initCarousel();
    }, 1500);
  });
  
  // 3. 照片轮播初始化
  const photos = [
    { src: "assets/photo_0.jpg", text: "手拿精美的北大红信封与毕业周边 🧧" },
    { src: "assets/photo_1.jpg", text: "坐在洒满阳光的教室内，安静而明媚 ☀️" },
    { src: "assets/photo_2.jpg", text: "元培学院典礼台上，庄严接过学位证书 🎓" },
    { src: "assets/photo_3.jpg", text: "与校长的亲切自拍合影，灿烂的笑脸 ✌️" },
    { src: "assets/photo_4.jpg", text: "未名湖畔相依，阳光落在我们的身上 💚" },
    { src: "assets/photo_5.jpg", text: "图书馆的书架旁，寻找属于未来的那一本书 📖" },
    { src: "assets/photo_6.jpg", text: "我是大富婆，他是我的小白脸！在图书馆的搞笑cosplay 🤭" },
    { src: "assets/photo_7.jpg", text: "图书馆前，向相机送出爱心 💖" }
  ];
  
  let currentSlide = 0;
  function initCarousel() {
    const inner = document.getElementById('carouselInner');
    inner.innerHTML = '';
    photos.forEach(p => {
      const slide = document.createElement('div');
      slide.className = 'carousel-slide';
      slide.innerHTML = `<img src="${p.src}"><div class="photo-caption">${p.text}</div>`;
      inner.appendChild(slide);
    });
  }
  
  function moveSlide(dir) {
    currentSlide += dir;
    if (currentSlide < 0) currentSlide = photos.length - 1;
    if (currentSlide >= photos.length) currentSlide = 0;
    const inner = document.getElementById('carouselInner');
    inner.style.transform = `translateX(-${currentSlide * (100 / photos.length)}%)`;
  }
  ```
  注意：CSS 照片轮播内部需要根据照片数量（8张）调整宽度。把 `carousel-inner` 宽度由 `800%` 改为动态计算或直接设为固定的百分比，`carousel-slide` 宽度为 `12.5%`（100 / 8 = 12.5）。

- [ ] **Step 4: 验证交互**
  - 验证拖拽：向左拖拽红点，红色流苏线能够倾斜偏转。滑到最左侧时，提示“拨穗完成”，并进入下一环节。
  - 验证扔帽：点击学士帽，学士帽缩小飞天，两侧同时飞出五彩纸屑喷泉，炫丽爆开，接着进入最终照片展示。
  - 验证相册：切换上一张下一张，照片 and 对应的定制文字描述正常加载。

- [ ] **Step 5: 验证交互**
  - 验证拖拽：向左拖拽红点，红色流苏线能够倾斜偏转。滑到最左侧时，提示“拨穗完成”，并进入下一环节。
  - 验证扔帽：点击学士帽，学士帽缩小飞天，两侧同时飞出五彩纸屑喷泉，炫丽爆开，接着进入最终照片展示。
  - 验证相册：切换上一张下一张，照片 and 对应的定制文字描述正常加载。

---

### Task 6: 移动端适配与最终整体验收

**Files:**
- Modify: `index.html`

**Interfaces:**
- Consumes: 全部 Task 的累加页面
- Produces: 最终稳定无 Bug、极佳视觉质感的一键运行 HTML 单文件项目。

- [ ] **Step 1: 全面优化样式与移动端适配细节**
  - 优化移动端双击放大问题（在 meta viewport 中已设置 `user-scalable=no`）。
  - 给所有的按钮和交互元素增加 `cursor: pointer` 并提供平滑的 `:active` 缩放微交互。
  - 检查字体大小、卡片最大宽度、各浏览器滚动条隐藏等体验细节，确保完美呈现。

- [ ] **Step 2: 实机在 iOS/Android Chrome/Safari 模拟器下调试**
  - 使用 Chrome DevTools 模拟各类机型（如 iPhone 14, Pixel 7 等）的长宽比。
  - 确保页面在短屏幕（如 SE）上不会被截断，在长屏幕上卡片居中显眼。

- [ ] **Step 3: 最终提交**
  ```bash
  git add index.html
  git commit -m "style: final mobile UI optimizations and polish"
  ```
