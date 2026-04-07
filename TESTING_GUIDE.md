# 测试指南 - Testing Guide

**项目**: 普法情景游戏 v1.4 (带图片支持)
**创建日期**: 2026-04-02
**状态**: ✅ Web播放器已创建，准备测试

---

## 📋 测试前准备清单

### ✅ 已完成的工作
- [x] 创建场景-图片映射文档 (SCENE_IMAGE_MAPPING.md)
- [x] 评估并选择实施方案 (IMAGE_INTEGRATION_SOLUTION.md)
- [x] 创建Tags技术规范 (TAGS_SPECIFICATION.md)
- [x] 在v1.4脚本中添加图片tags (普法情景游戏_v1.4_with_tags.ink)
- [x] 创建Web播放器 (web_player.html)

### 🎯 本次测试目标
1. 验证Web播放器能正确加载故事
2. 确认图片按tags正确显示
3. 测试角色切换和背景变化
4. 检查特效和滤镜效果
5. 测试情绪边框颜色变化

---

## 📦 Step 1: 导出Ink脚本为JSON

### 方法A: 使用Inky编辑器 (推荐)

1. **打开Inky编辑器**
   - 打开 `普法情景游戏_v1.4_with_tags.ink`

2. **导出为Web**
   ```
   菜单栏 → File → Export → Export story.js for web...
   ```

3. **保存文件**
   - 文件名: `story.json` (重要！必须是这个名字)
   - 保存位置: `/Users/liyuelin/Desktop/Claude/inky/`
   - 确认保存在项目根目录

### 方法B: 使用Inklecate命令行 (备选)

如果你安装了inklecate命令行工具：

```bash
cd /Users/liyuelin/Desktop/Claude/inky
inklecate -o story.json 普法情景游戏_v1.4_with_tags.ink
```

---

## 📥 Step 2: 获取Ink Runtime (ink.js)

### 选项1: 从GitHub下载 (推荐)

1. **访问Ink官方仓库**
   ```
   https://github.com/inkle/ink/releases
   ```

2. **下载ink.js**
   - 找到最新release
   - 下载 `ink.js` 文件
   - 保存到项目根目录: `/Users/liyuelin/Desktop/Claude/inky/ink.js`

### 选项2: 使用CDN (临时测试)

如果只是快速测试，可以修改 `web_player.html` 第290行：

```html
<!-- 原来的 -->
<script src="ink.js"></script>

<!-- 改为 -->
<script src="https://cdn.jsdelivr.net/npm/inkjs@2.2.3/dist/ink.js"></script>
```

**注意**: CDN方式需要网络连接，正式发布建议使用本地文件。

---

## 📁 Step 3: 检查文件结构

确保你的项目目录结构如下：

```
/Users/liyuelin/Desktop/Claude/inky/
├── web_player.html           # ✅ Web播放器
├── story.json                # ⏳ 需要从Step 1导出
├── ink.js                    # ⏳ 需要从Step 2获取
├── graphics/                 # ✅ 图片文件夹
│   ├── background/
│   │   ├── 放学路_第一幕主背景.jpg
│   │   ├── 孙伯伯家院子.jpg
│   │   ├── 孙伯伯家里屋.jpg
│   │   └── 河边树林外.png
│   └── characters/
│       ├── 小雨平静版.png
│       ├── 小雨害怕版1.png
│       ├── 小雨害怕版2.png
│       ├── 孙伯伯和蔼版.png
│       ├── 孙伯伯阴沉版.png
│       └── 女警.png
└── 普法情景游戏_v1.4_with_tags.ink  # ✅ 源脚本
```

### 验证文件存在

在终端运行：

```bash
cd /Users/liyuelin/Desktop/Claude/inky
ls -lh web_player.html story.json ink.js
ls -lh graphics/background/
ls -lh graphics/characters/
```

---

## 🚀 Step 4: 启动本地服务器

**重要**: 由于浏览器安全限制，必须通过HTTP服务器访问，不能直接双击HTML文件。

### 方法A: 使用Python (推荐 - macOS自带)

```bash
cd /Users/liyuelin/Desktop/Claude/inky

# Python 3
python3 -m http.server 8000

# 或者 Python 2
python -m SimpleHTTPServer 8000
```

### 方法B: 使用Node.js http-server

```bash
# 全局安装 (只需一次)
npm install -g http-server

# 启动服务器
cd /Users/liyuelin/Desktop/Claude/inky
http-server -p 8000
```

### 方法C: 使用PHP (macOS自带)

```bash
cd /Users/liyuelin/Desktop/Claude/inky
php -S localhost:8000
```

---

## 🌐 Step 5: 在浏览器中测试

### 打开游戏

1. **启动浏览器**
   - 推荐: Chrome, Safari, Firefox

2. **访问地址**
   ```
   http://localhost:8000/web_player.html
   ```

3. **预期看到**
   - 游戏容器（白色卡片）
   - 顶部图片显示区域（紫色渐变背景，400px高）
   - 底部文本和选项区域
   - "正在加载游戏..." 提示

### 如果加载成功

- 提示消失，显示第一幕内容
- 背景图片: "放学路_第一幕主背景.jpg"
- 左侧角色: 小雨平静版
- 右侧角色: 孙伯伯和蔼版
- 蓝色顶部边框 (mood: calm)

---

## 🧪 Step 6: 测试核心功能

### 测试1: 背景图片切换

1. **第一幕**: 应显示"放学路"背景
2. **选择"跟着孙伯伯走"** → 进入第二幕
3. **第二幕**: 背景应切换为"孙伯伯家院子.jpg"

**预期**: 图片平滑切换，有淡入动画

---

### 测试2: 角色状态变化

1. **在院子场景**: 小雨应该是"平静版"
2. **选择"留在孙伯伯家"** → 进入紧张场景
3. **里屋场景**: 小雨应切换为"害怕版1"，孙伯伯切换为"阴沉版"

**预期**: 角色图片正确切换，情绪边框变为橙色 (tense)

---

### 测试3: 危险场景效果

1. **进入危险场景** (act3A_stage2)
2. **预期效果**:
   - 背景保持"里屋"
   - 屏幕抖动效果 (effect: shake)
   - 边框变为红色 (mood: danger)

---

### 测试4: 侵害场景滤镜

1. **触发侵害场景** (assault_scene)
2. **预期效果**:
   - 背景变暗 (bg_filter: darken)
   - 只显示中央角色 (小雨害怕版2)
   - 左右角色消失
   - 红色边框 (mood: danger)

---

### 测试5: 警察调查场景

1. **选择报警** → 进入police_investigation
2. **预期效果**:
   - 背景消失 (bg: none)
   - 左侧: 小雨害怕版1
   - 右侧: 女警
   - 绿色边框 (mood: safe)

---

### 测试6: 结局场景

1. **到达结局A2** (ending_A2_assault_reported)
2. **预期效果**:
   - 背景: 河边树林外
   - 紫色边框 (mood: hopeful)
   - 结局文本显示

---

## 🐛 常见问题排查

### 问题1: "正在加载游戏..." 一直显示

**可能原因**:
- `story.json` 文件不存在或路径错误
- `ink.js` 文件缺失
- 没有通过HTTP服务器访问

**解决方法**:
1. 打开浏览器开发者工具 (F12 或 Cmd+Option+I)
2. 查看Console标签页错误信息
3. 检查Network标签页，确认所有文件加载成功

---

### 问题2: 图片不显示

**可能原因**:
- 图片文件路径错误
- 图片文件名拼写错误（区分大小写）
- graphics文件夹位置不对

**解决方法**:
```bash
# 检查图片文件
ls -lh graphics/background/
ls -lh graphics/characters/

# 确保文件名完全匹配tags中的名称
```

---

### 问题3: 控制台显示 "404 Not Found"

**错误示例**:
```
GET http://localhost:8000/story.json 404 (Not Found)
```

**解决方法**:
- 确认 `story.json` 在项目根目录
- 确认当前工作目录正确
- 重新导出Ink脚本

---

### 问题4: 图片路径错误

**错误示例**:
```
GET http://localhost:8000/graphics/background/放学路_第一幕主背景.jpg 404
```

**解决方法**:
1. 检查文件名编码（中文文件名）
2. 确认graphics文件夹结构
3. 可以临时改用英文文件名测试

---

## 📊 测试检查表

完成以下检查项：

### 基础功能
- [ ] Web播放器正常加载
- [ ] 故事文本正确显示
- [ ] 选项按钮可点击
- [ ] 游戏逻辑正常推进

### 图片显示
- [ ] 背景图片正确显示
- [ ] 背景切换正常（放学路 → 院子 → 里屋 → 树林）
- [ ] 左侧角色显示正确
- [ ] 右侧角色显示正确
- [ ] 中央角色显示正确 (侵害场景)
- [ ] 角色状态切换正确 (平静 → 害怕1 → 害怕2)

### 视觉效果
- [ ] mood边框颜色正确 (calm=蓝, safe=绿, tense=橙, danger=红)
- [ ] shake效果正常 (屏幕抖动)
- [ ] fade效果正常 (淡入)
- [ ] darken滤镜正常 (背景变暗)

### 响应式设计
- [ ] 桌面浏览器显示正常
- [ ] 移动设备显示正常 (可选)
- [ ] 文本区域可滚动 (长文本)

---

## 📸 截图建议

为了验证效果，建议截图以下场景：

1. **第一幕** - 放学路场景 (蓝色边框)
2. **第二幕** - 院子场景 (角色完整显示)
3. **危险场景** - 里屋紧张气氛 (橙色/红色边框)
4. **侵害场景** - 变暗效果 (只有中央角色)
5. **警察场景** - 女警出现 (绿色边框)
6. **结局场景** - 树林背景 (紫色边框)

---

## 🔧 调试技巧

### 查看Tags输出

在浏览器Console中输入：

```javascript
// 查看当前tags
console.log(story.currentTags);

// 查看当前文本
console.log(story.currentText);
```

### 手动触发效果

```javascript
// 测试背景切换
updateBackground('孙伯伯家院子.jpg');

// 测试角色显示
updateCharacter('left', '小雨害怕版1.png');

// 测试特效
triggerEffect('shake');

// 测试滤镜
applyBackgroundFilter('darken');
```

---

## ✅ 测试完成标准

当所有以下条件满足时，视为测试通过：

1. ✅ 游戏可以从头玩到至少一个结局
2. ✅ 所有10张图片都能正确显示
3. ✅ 至少测试了6个核心场景的图片切换
4. ✅ mood边框颜色在不同场景正确变化
5. ✅ 至少一个特效正常工作 (shake/fade)
6. ✅ 没有浏览器控制台报错

---

## 📝 下一步计划

测试通过后：

### Phase 2: 完整实现
- [ ] 为剩余场景添加tags (v1.4目前7个场景，共约17个场景)
- [ ] 增强过渡动画效果
- [ ] 添加图片预加载功能
- [ ] 优化移动端显示

### Phase 3: 打磨
- [ ] 添加音效支持 (可选)
- [ ] 添加loading进度条
- [ ] 多浏览器兼容性测试
- [ ] 性能优化

### 发布准备
- [ ] 创建独立发布包
- [ ] 编写用户使用说明
- [ ] 准备演示视频/截图

---

## 📞 需要帮助？

如果测试中遇到问题：

1. **检查浏览器Console** - 90%的问题会有错误提示
2. **确认文件结构** - 使用本文档的检查清单
3. **查看TAGS_SPECIFICATION.md** - 确认tags语法正确
4. **参考IMAGE_INTEGRATION_SOLUTION.md** - 了解实现原理

---

**祝测试顺利！🎮**

如果所有功能正常，你的普法情景游戏将成为一个带精美图片的互动小说！
