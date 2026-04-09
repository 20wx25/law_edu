# AI小助手 Implementation Plan

> **For Claude:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在普法情景游戏右侧添加一个AI守护精灵助手侧边栏，能感知游戏状态，基于法律知识库准确引用法律条文回答儿童玩家的问题。

**Architecture:** 纯前端方案，无需后端。新增 `legal_knowledge.json` 存放法律条文，页面改为左右双栏布局，右侧侧边栏内嵌聊天UI。每次用户发问时，前端做关键词匹配检索最相关条文，连同当前游戏状态一起注入 system prompt，调用硅基流动 API（OpenAI 兼容，流式输出）。

**Tech Stack:** 原生 HTML/CSS/JS，硅基流动 API（Qwen/Qwen3.5-397B-A17B），inkjs

---

## 文件结构

| 文件 | 操作 | 职责 |
|------|------|------|
| `legal_knowledge.json` | 新建 | 法律条文知识库，按法律名称+条款编号组织 |
| `web_player.html` | 修改 | 布局改为双栏，右侧加侧边栏+聊天逻辑+API调用+知识库检索 |

---

## Chunk 1: 法律知识库

### Task 1: 创建 legal_knowledge.json

**Files:**
- Create: `legal_knowledge.json`

- [ ] **Step 1: 创建知识库文件**

创建 `legal_knowledge.json`，包含与游戏主题（未成年人保护、性侵害防范）高度相关的法律条文。

```json
[
  {
    "id": "未成年人保护法-1",
    "law": "《中华人民共和国未成年人保护法》",
    "article": "第三条",
    "content": "国家保障未成年人的生存权、发展权、受保护权、参与权等权利。未成年人依法平等地享有各项权利，不因本人及其父母或者其他监护人的民族、种族、性别、户籍、职业、宗教信仰、教育程度、家庭状况、身心健康状况等受到歧视。",
    "keywords": ["权利", "保护", "未成年人", "权益"]
  },
  {
    "id": "未成年人保护法-2",
    "law": "《中华人民共和国未成年人保护法》",
    "article": "第十一条",
    "content": "任何组织或者个人发现不利于未成年人身心健康或者侵犯未成年人合法权益的情形，都有权劝阻、制止或者向公安、民政、教育等有关部门提出检举、控告。国家机关、居民委员会、村民委员会、密切接触未成年人的单位及其工作人员，发现未成年人身心健康受到侵害、疑似受到侵害或者面临其他危险情形的，应当立即向公安、民政、教育等有关部门报告。",
    "keywords": ["举报", "报告", "检举", "控告", "发现", "侵害"]
  },
  {
    "id": "未成年人保护法-3",
    "law": "《中华人民共和国未成年人保护法》",
    "article": "第二十二条",
    "content": "未成年人的父母或者其他监护人应当预防和制止未成年人遭受性侵害、性骚扰。",
    "keywords": ["性侵害", "性骚扰", "预防", "监护人", "父母"]
  },
  {
    "id": "未成年人保护法-4",
    "law": "《中华人民共和国未成年人保护法》",
    "article": "第六十二条",
    "content": "禁止对未成年人实施性侵害、性骚扰。学校、幼儿园应当建立预防性侵害、性骚扰未成年人的工作制度，建立及早发现、及时处置的相关机制，将性教育纳入学校、幼儿园教育内容。",
    "keywords": ["性侵害", "性骚扰", "学校", "禁止", "教育", "保护"]
  },
  {
    "id": "未成年人保护法-5",
    "law": "《中华人民共和国未成年人保护法》",
    "article": "第一百零六条",
    "content": "对未成年人实施性侵害、性骚扰的，由公安机关依法给予行政拘留，并处罚款；构成犯罪的，依法追究刑事责任。",
    "keywords": ["性侵害", "性骚扰", "处罚", "拘留", "刑事责任", "犯罪"]
  },
  {
    "id": "刑法-1",
    "law": "《中华人民共和国刑法》",
    "article": "第二百三十六条",
    "content": "以暴力、胁迫或者其他手段强奸妇女的，处三年以上十年以下有期徒刑。奸淫不满十四周岁的幼女的，以强奸论，从重处罚。强奸妇女、奸淫幼女，有下列情形之一的，处十年以上有期徒刑、无期徒刑或者死刑：（一）强奸妇女、奸淫幼女情节恶劣的；（二）强奸多人的；（三）在公共场所当众强奸的；（四）二人以上轮奸的；（五）强奸后杀人的；（六）致使被害人重伤、死亡或者造成其他严重后果的。",
    "keywords": ["强奸", "幼女", "性侵", "有期徒刑", "犯罪", "处罚", "十四周岁"]
  },
  {
    "id": "刑法-2",
    "law": "《中华人民共和国刑法》",
    "article": "第二百三十七条",
    "content": "以暴力、胁迫或者其他方法强制猥亵他人或者侮辱妇女的，处五年以下有期徒刑或者拘役。聚众或者在公共场所当众犯前款罪的，或者有其他恶劣情节的，处五年以上有期徒刑。猥亵儿童的，依照前两款的规定从重处罚。",
    "keywords": ["猥亵", "强制", "侮辱", "儿童", "处罚", "犯罪", "有期徒刑"]
  },
  {
    "id": "刑法-3",
    "law": "《中华人民共和国刑法》",
    "article": "第二百三十四条",
    "content": "故意伤害他人身体的，处三年以下有期徒刑、拘役或者管制。犯前款罪，致人重伤的，处三年以上十年以下有期徒刑；致人死亡或者以特别残忍手段致人重伤造成严重残疾的，处十年以上有期徒刑、无期徒刑或者死刑。",
    "keywords": ["伤害", "故意伤害", "身体", "犯罪", "处罚"]
  },
  {
    "id": "刑法-4",
    "law": "《中华人民共和国刑法》",
    "article": "第十七条",
    "content": "已满十六周岁的人犯罪，应当负刑事责任。已满十四周岁不满十六周岁的人，犯故意杀人、故意伤害致人重伤或者死亡、强奸、抢劫、贩卖毒品、放火、爆炸、投毒罪的，应当负刑事责任。已满十二周岁不满十四周岁的人，犯故意杀人、故意伤害罪，致人死亡或者以特别残忍手段致人重伤造成严重残疾，情节恶劣，经最高人民检察院核准追诉的，应当负刑事责任。",
    "keywords": ["刑事责任", "年龄", "未成年", "十四周岁", "十六周岁", "犯罪年龄"]
  },
  {
    "id": "民法典-1",
    "law": "《中华人民共和国民法典》",
    "article": "第一千零一条",
    "content": "自然人享有生命权、身体权、健康权、姓名权、肖像权、名誉权、荣誉权、隐私权、婚姻自主权等权利。",
    "keywords": ["身体权", "健康权", "隐私权", "权利", "自然人"]
  },
  {
    "id": "民法典-2",
    "law": "《中华人民共和国民法典》",
    "article": "第一千零三十二条",
    "content": "自然人享有隐私权。任何组织或者个人不得以刺探、侵扰、泄露、公开等方式侵害他人的隐私权。隐私是自然人的私人生活安宁和不愿为他人知晓的私密空间、私密活动、私密信息。",
    "keywords": ["隐私权", "隐私", "侵害", "私密", "保护"]
  },
  {
    "id": "预防未成年人犯罪法-1",
    "law": "《中华人民共和国预防未成年人犯罪法》",
    "article": "第四十一条",
    "content": "国家对遭受侵害的未成年人给予特殊保护，采取措施及时救助、妥善安置遭受侵害的未成年人，并给予相应的心理援助。",
    "keywords": ["救助", "保护", "侵害", "心理援助", "安置"]
  },
  {
    "id": "妇女权益保障法-1",
    "law": "《中华人民共和国妇女权益保障法》",
    "article": "第二十三条",
    "content": "禁止对妇女实施性骚扰。受害妇女可以向有关单位和国家机关投诉。接到投诉的有关单位和国家机关应当及时处理，不得推诿。",
    "keywords": ["性骚扰", "妇女", "投诉", "禁止", "处理"]
  },
  {
    "id": "儿童权利公约-1",
    "law": "《联合国儿童权利公约》",
    "article": "第十九条",
    "content": "缔约国应采取一切适当的立法、行政、社会和教育措施，保护儿童在受父母、法定监护人或其他任何负责照管儿童的人的照料时，不致遭受任何形式的身心摧残、伤害或凌辱，忽视或照料不周，虐待或剥削，包括性侵犯。",
    "keywords": ["儿童", "性侵犯", "保护", "虐待", "国际法"]
  },
  {
    "id": "报警-1",
    "law": "常识",
    "article": "报警须知",
    "content": "中国大陆报警电话：110（警察）。遭受侵害时应尽快拨打110报警，告知自己的位置、侵害人信息。保留证据（衣物、信息、证人）。告诉可信任的大人（父母、老师）。",
    "keywords": ["报警", "110", "求救", "怎么办", "求助", "帮助"]
  },
  {
    "id": "证据-1",
    "law": "常识",
    "article": "证据保护须知",
    "content": "遭受侵害后，应注意保护证据：不要洗澡、不要换洗衣物、保留相关物品、记录时间地点经过、记下侵害人特征。尽快就医，由医生出具证明。向警方报告时提供尽可能多的细节。",
    "keywords": ["证据", "保留证据", "报案", "怎么做", "被侵害后"]
  }
]
```

- [ ] **Step 2: 确认文件已创建**

检查 `legal_knowledge.json` 存在且 JSON 格式合法（可在浏览器控制台运行 `JSON.parse()` 验证）。

- [ ] **Step 3: Commit**

```bash
git add legal_knowledge.json
git commit -m "feat: add legal knowledge base for AI assistant"
```

---

## Chunk 2: 页面布局改造

### Task 2: 修改 web_player.html — 双栏布局

**Files:**
- Modify: `web_player.html`（`<body>` 结构 + CSS）

- [ ] **Step 1: 修改 body 布局**

将 `<body>` 内的 `<div id="game-container">` 包裹在一个新的外层容器中，右边添加侧边栏：

```html
<body>
  <div id="page-wrapper">
    <!-- 左栏：原游戏（完全不变） -->
    <div id="game-container">
      <!-- ... 原有内容不变 ... -->
    </div>

    <!-- 右栏：AI助手侧边栏 -->
    <div id="ai-sidebar">
      <div id="ai-header">
        <span id="ai-avatar">✨</span>
        <div id="ai-title-group">
          <span id="ai-name">灵灵</span>
          <span id="ai-subtitle">你的守护精灵</span>
        </div>
      </div>
      <div id="ai-messages"></div>
      <div id="ai-input-area">
        <textarea id="ai-input" placeholder="问灵灵任何问题..." rows="2"></textarea>
        <button id="ai-send">发送</button>
      </div>
    </div>
  </div>
</body>
```

- [ ] **Step 2: 添加 CSS — 双栏布局和侧边栏样式**

在 `<style>` 标签末尾（`</style>` 之前）添加以下样式：

```css
/* ===== 双栏布局 ===== */
body {
  /* 覆盖原有的 align-items: center，让两栏顶部对齐 */
  align-items: flex-start;
}

#page-wrapper {
  display: flex;
  flex-direction: row;
  gap: 20px;
  align-items: flex-start;
  width: 100%;
  max-width: 1260px;
  margin: 0 auto;
}

/* 原 game-container 不改宽度，flex-shrink 防止压缩 */
#game-container {
  flex-shrink: 0;
}

/* ===== AI侧边栏 ===== */
#ai-sidebar {
  width: 320px;
  flex-shrink: 0;
  background: white;
  border-radius: 15px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  height: 800px; /* 与游戏容器等高 */
  overflow: hidden;
  position: sticky;
  top: 20px;
}

#ai-header {
  padding: 16px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  display: flex;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
}

#ai-avatar {
  font-size: 32px;
  line-height: 1;
}

#ai-title-group {
  display: flex;
  flex-direction: column;
}

#ai-name {
  font-size: 18px;
  font-weight: bold;
}

#ai-subtitle {
  font-size: 12px;
  opacity: 0.85;
}

#ai-messages {
  flex: 1;
  overflow-y: auto;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  background: #f8f8fc;
}

/* 消息气泡 */
.ai-msg {
  max-width: 85%;
  padding: 10px 14px;
  border-radius: 12px;
  font-size: 14px;
  line-height: 1.7;
  word-break: break-word;
  white-space: pre-wrap;
}

.ai-msg.assistant {
  background: white;
  border: 1px solid #e8e8f0;
  align-self: flex-start;
  border-bottom-left-radius: 4px;
}

.ai-msg.user {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  align-self: flex-end;
  border-bottom-right-radius: 4px;
}

/* 打字动画气泡 */
.ai-msg.typing {
  background: white;
  border: 1px solid #e8e8f0;
  align-self: flex-start;
  border-bottom-left-radius: 4px;
  color: #999;
  font-style: italic;
}

/* 法律引用标注 */
.legal-cite {
  margin-top: 8px;
  padding: 6px 10px;
  background: #f0f4ff;
  border-left: 3px solid #667eea;
  border-radius: 0 6px 6px 0;
  font-size: 12px;
  color: #555;
}

#ai-input-area {
  padding: 12px;
  border-top: 1px solid #e8e8f0;
  display: flex;
  gap: 8px;
  flex-shrink: 0;
  background: white;
}

#ai-input {
  flex: 1;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 8px 12px;
  font-size: 14px;
  font-family: inherit;
  resize: none;
  outline: none;
  transition: border-color 0.2s;
}

#ai-input:focus {
  border-color: #667eea;
}

#ai-send {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 0 16px;
  font-size: 14px;
  cursor: pointer;
  transition: opacity 0.2s;
  white-space: nowrap;
}

#ai-send:hover {
  opacity: 0.85;
}

#ai-send:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* 响应式：小屏幕隐藏侧边栏 */
@media (max-width: 1100px) {
  #page-wrapper {
    flex-direction: column;
    align-items: center;
  }

  #ai-sidebar {
    width: 100%;
    max-width: 900px;
    height: 400px;
    position: static;
  }
}

/* 滚动条 */
#ai-messages::-webkit-scrollbar {
  width: 6px;
}
#ai-messages::-webkit-scrollbar-track {
  background: transparent;
}
#ai-messages::-webkit-scrollbar-thumb {
  background: #ccc;
  border-radius: 3px;
}
```

- [ ] **Step 3: 在浏览器中验证布局**

用本地服务器打开页面，确认：
- 游戏区域在左，侧边栏在右
- 两栏并排，侧边栏高度与游戏区域相近
- 响应式在窄屏时变为上下布局

---

## Chunk 3: AI 逻辑

### Task 3: 添加 AI 配置、游戏状态追踪、知识库检索、API调用

**Files:**
- Modify: `web_player.html`（`<script>` 部分）

- [ ] **Step 1: 在现有 `<script>` 开头添加 AI 角色配置块**

在 `let story;` 这行**之前**插入：

```js
// ============================================================
// AI 小助手配置（如需修改角色设定，在此处编辑）
// ============================================================
const AI_CONFIG = {
  // 助手名字（显示在侧边栏顶部）
  name: "灵灵",

  // 头像（emoji 或图片路径，如 "graphics/characters/灵灵.png"）
  avatar: "✨",

  // 副标题
  subtitle: "你的守护精灵",

  // 硅基流动 API
  apiBase: "https://api.siliconflow.cn/v1",
  apiKey: "sk-dzzhzvawwfvnxsgvlgqobshghylhptadvszbdqzhcolcynrz",
  model: "Qwen/Qwen3.5-397B-A17B",

  // 角色系统提示词（修改此处调整助手的说话风格和人格）
  systemPromptBase: `你是"灵灵"，一个温暖、善良的守护精灵，专门陪伴小朋友学习法律知识和自我保护。

你的说话风格：
- 用简单、友善的语气，像一个温暖的大姐姐或大哥哥
- 解释法律时用小朋友能理解的方式，避免太多法律术语
- 遇到敏感话题时，先给予情感支持，再给出实际建议
- 鼓励小朋友勇于求助，告诉可信任的大人

当引用法律条文时：
- 必须准确标注法律名称和条款编号
- 在回答末尾用"法律依据："列出引用来源
- 只引用提供给你的条文原文，不要编造条款`
};

// ============================================================
// 游戏状态（由游戏逻辑自动更新，AI 问答时自动注入上下文）
// ============================================================
const gameState = {
  currentBg: "",        // 当前背景场景
  currentMood: "",      // 当前情绪氛围
  harmValue: null,      // Harm 变量值
  supportValue: null,   // Support 变量值
  recentText: [],       // 最近3段故事文本（滚动更新）
  lastChoice: ""        // 玩家最后一次做出的选择
};
```

- [ ] **Step 2: 修改 `continueStory()` 函数，在其中更新游戏状态**

找到 `function continueStory()` 函数，在 `continueStory` 调用 `displayText(paragraphText)` **之前**插入：

```js
// 更新游戏状态供AI使用
if (paragraphText.trim()) {
  gameState.recentText.push(paragraphText.trim().slice(0, 200));
  if (gameState.recentText.length > 3) gameState.recentText.shift();
}
// 尝试读取 Ink 变量
try {
  gameState.harmValue = story.variablesState["Harm"];
  gameState.supportValue = story.variablesState["Support"];
} catch(e) {}
```

- [ ] **Step 3: 修改 `updateBackground()` 和 `updateMood()`，同步更新 gameState**

在 `updateBackground(filename)` 函数内，`bg.src = ...` 这行**之前**插入：

```js
gameState.currentBg = filename;
```

在 `updateMood(mood)` 函数内，`textArea.classList.add(...)` 这行**之前**插入：

```js
gameState.currentMood = mood;
```

在 `displayChoices()` 函数内，`button.onclick` 里 `story.ChooseChoiceIndex(index)` **之前**插入：

```js
gameState.lastChoice = choice.text;
```

- [ ] **Step 4: 添加法律知识库加载和检索函数**

在 `window.onload = initGame;` **之前**插入：

```js
// ===== 法律知识库 =====
let legalKnowledge = [];

async function loadLegalKnowledge() {
  try {
    const res = await fetch('legal_knowledge.json');
    legalKnowledge = await res.json();
  } catch (e) {
    console.warn("法律知识库加载失败:", e);
  }
}

function searchLegalKnowledge(query) {
  if (!legalKnowledge.length) return [];
  const q = query.toLowerCase();
  const scored = legalKnowledge.map(item => {
    let score = 0;
    item.keywords.forEach(kw => {
      if (q.includes(kw)) score += 2;
    });
    if (q.includes(item.law)) score += 3;
    // 简单词频匹配
    const words = q.split(/[\s，。？！、]/);
    words.forEach(w => {
      if (w.length > 1 && item.content.includes(w)) score += 1;
    });
    return { item, score };
  });
  return scored
    .filter(s => s.score > 0)
    .sort((a, b) => b.score - a.score)
    .slice(0, 3)
    .map(s => s.item);
}
```

- [ ] **Step 5: 添加 AI 聊天核心函数（流式调用）**

继续在 `window.onload = initGame;` **之前**插入：

```js
// ===== AI 聊天 =====
const chatHistory = [];   // 保存对话历史（不含 system）

function buildSystemPrompt(userQuery) {
  // 检索相关法律条文
  const relatedLaws = searchLegalKnowledge(userQuery);
  let lawContext = "";
  if (relatedLaws.length > 0) {
    lawContext = "\n\n【相关法律条文参考（请在回答中准确引用）】\n";
    relatedLaws.forEach(law => {
      lawContext += `\n${law.law} ${law.article}：\n"${law.content}"\n`;
    });
  }

  // 构建游戏状态上下文
  let stateContext = "\n\n【当前游戏状态（帮助你理解玩家的处境）】\n";
  if (gameState.currentBg) stateContext += `- 当前场景：${gameState.currentBg}\n`;
  if (gameState.currentMood) stateContext += `- 当前氛围：${gameState.currentMood}\n`;
  if (gameState.harmValue !== null) stateContext += `- 伤害值(Harm)：${gameState.harmValue}\n`;
  if (gameState.supportValue !== null) stateContext += `- 支持值(Support)：${gameState.supportValue}\n`;
  if (gameState.lastChoice) stateContext += `- 玩家最近选择：${gameState.lastChoice}\n`;
  if (gameState.recentText.length > 0) {
    stateContext += `- 最近剧情片段：\n"${gameState.recentText.join('\n...\n')}"\n`;
  }

  return AI_CONFIG.systemPromptBase + lawContext + stateContext;
}

async function sendToAI(userMessage) {
  const sendBtn = document.getElementById('ai-send');
  const input = document.getElementById('ai-input');
  sendBtn.disabled = true;
  input.disabled = true;

  // 显示用户消息
  appendMessage('user', userMessage);

  // 显示打字动画
  const typingEl = appendMessage('typing', '灵灵正在思考...');

  // 准备请求
  const messages = [
    { role: "system", content: buildSystemPrompt(userMessage) },
    ...chatHistory,
    { role: "user", content: userMessage }
  ];

  try {
    const res = await fetch(`${AI_CONFIG.apiBase}/chat/completions`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${AI_CONFIG.apiKey}`
      },
      body: JSON.stringify({
        model: AI_CONFIG.model,
        messages,
        stream: true,
        max_tokens: 800,
        temperature: 0.7
      })
    });

    if (!res.ok) {
      throw new Error(`API 错误: ${res.status}`);
    }

    // 流式读取
    const reader = res.body.getReader();
    const decoder = new TextDecoder();
    let fullText = "";

    // 替换打字动画为真实气泡
    typingEl.className = 'ai-msg assistant';
    typingEl.textContent = "";

    while (true) {
      const { done, value } = await reader.read();
      if (done) break;

      const chunk = decoder.decode(value);
      const lines = chunk.split('\n').filter(l => l.startsWith('data: '));

      for (const line of lines) {
        const data = line.slice(6);
        if (data === '[DONE]') break;
        try {
          const json = JSON.parse(data);
          const delta = json.choices?.[0]?.delta?.content || "";
          fullText += delta;
          typingEl.textContent = fullText;
          // 自动滚动到底部
          const msgs = document.getElementById('ai-messages');
          msgs.scrollTop = msgs.scrollHeight;
        } catch(e) {}
      }
    }

    // 保存到对话历史
    chatHistory.push({ role: "user", content: userMessage });
    chatHistory.push({ role: "assistant", content: fullText });
    // 最多保留最近10轮
    if (chatHistory.length > 20) chatHistory.splice(0, 2);

  } catch (err) {
    typingEl.className = 'ai-msg assistant';
    typingEl.textContent = `出错了：${err.message}。请检查网络连接或稍后再试。`;
  }

  sendBtn.disabled = false;
  input.disabled = false;
  input.focus();
}

function appendMessage(role, text) {
  const msgs = document.getElementById('ai-messages');
  const el = document.createElement('div');
  el.className = `ai-msg ${role}`;
  el.textContent = text;
  msgs.appendChild(el);
  msgs.scrollTop = msgs.scrollHeight;
  return el;
}

function initAISidebar() {
  // 更新 header 信息
  document.getElementById('ai-avatar').textContent = AI_CONFIG.avatar;
  document.getElementById('ai-name').textContent = AI_CONFIG.name;
  document.getElementById('ai-subtitle').textContent = AI_CONFIG.subtitle;

  // 发送欢迎消息
  appendMessage('assistant',
    `你好！我是${AI_CONFIG.name}，你的守护精灵 ✨\n\n在游戏过程中，你可以随时问我任何问题——关于法律、安全，或者刚才的故事情节。我会尽力帮助你！`
  );

  // 绑定发送按钮
  document.getElementById('ai-send').addEventListener('click', handleSend);

  // 回车发送（Shift+Enter 换行）
  document.getElementById('ai-input').addEventListener('keydown', e => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleSend();
    }
  });
}

function handleSend() {
  const input = document.getElementById('ai-input');
  const text = input.value.trim();
  if (!text) return;
  input.value = "";
  sendToAI(text);
}
```

- [ ] **Step 6: 修改 `window.onload` 初始化**

将末尾的：
```js
window.onload = initGame;
```

替换为：
```js
window.onload = async function() {
  await loadLegalKnowledge();
  initAISidebar();
  initGame();
};
```

- [ ] **Step 7: 在浏览器中测试**

启动本地服务器：
```bash
cd D:/project/jianqiao/law_edu
python -m http.server 8080
```

打开 `http://localhost:8080/web_player.html`，验证：
1. 侧边栏出现在游戏右侧
2. 欢迎消息正确显示
3. 输入问题并发送，能收到流式回复
4. 询问法律问题（如"猥亵儿童是犯罪吗"），确认 AI 正确引用了法律条文
5. 游戏进行几步后再问问题，确认 AI 能提到当前场景信息

- [ ] **Step 8: Commit**

```bash
git add web_player.html
git commit -m "feat: add AI guardian assistant sidebar with legal knowledge RAG"
```

---

## 完成清单

- [ ] `legal_knowledge.json` 创建完毕，包含 16 条法律条文
- [ ] 页面改为左右双栏布局，原游戏内容不变
- [ ] 侧边栏显示正常，欢迎消息可见
- [ ] AI 能正常回答问题（流式输出）
- [ ] AI 引用法律时能准确标注来源
- [ ] AI 回答能感知当前游戏状态（场景、选择）
- [ ] 响应式设计：小屏幕自动变为上下布局
- [ ] 角色配置在 `web_player.html` 顶部 `AI_CONFIG` 对象中，注释清晰
