# Ink Tags 图片标签规范文档

**项目**: 普法情景游戏
**版本**: 1.0
**创建日期**: 2026-04-02
**适用于**: v1.3及后续版本

---

## 一、Tags系统概述

### 什么是Tags？

Tags是Ink原生支持的元数据标记系统，用`#`符号标记，不会显示给玩家，但可以被游戏引擎读取。

**官方用途** (来自`WritingWithInk.md`):
> "用于图形冒险游戏中，根据角色表情显示不同的美术资源"

### 本项目Tags用途

在本项目中，Tags用于控制:
1. **背景图片** - 场景背景
2. **角色图片** - 左中右三个位置的角色
3. **视觉效果** - 滤镜、特效、过渡动画
4. **情绪氛围** - 用于CSS样式切换

---

## 二、Tags语法规则

### 基础语法

```ink
# 标签名: 标签值
```

**重要规则**:
- `#` 后面有一个空格
- 标签名和值之间用`: `分隔（冒号后有空格）
- 一行可以有多个tags，每个tag用`#`分隔

**示例**:
```ink
# bg: 放学路_第一幕主背景.jpg
# char_left: 小雨平静版.png # char_right: 孙伯伯和蔼版.png
```

### Tags位置规则

**方式1: 标签在内容上方** (推荐)
```ink
=== act1_after_school ===
# bg: 放学路_第一幕主背景.jpg
# char_left: 小雨平静版.png
# char_right: 孙伯伯和蔼版.png

放学铃声响起，同学们陆续离开教室。
```

**方式2: 标签在内容同行**
```ink
放学铃声响起，同学们陆续离开教室。 # bg: 放学路_第一幕主背景.jpg
```

**方式3: 混合使用**
```ink
# bg: 放学路_第一幕主背景.jpg
# char_left: 小雨平静版.png

放学铃声响起，同学们陆续离开教室。 # mood: calm
```

**推荐**: 场景级tags（背景、角色）放在knot/stitch开头，行为级tags（情绪、特效）放在具体对话行

---

## 三、标准Tags定义

### 1. 背景图片 (Background)

**标签名**: `bg`
**功能**: 设置当前场景的背景图片

#### 语法

```ink
# bg: 文件名.jpg
```

#### 可用值

| 文件名 | 场景描述 | 尺寸 |
|--------|---------|------|
| `放学路_第一幕主背景.jpg` | 放学路上，乡村小路 | 183 KB |
| `孙伯伯家院子.jpg` | 孙伯伯家院子，堂屋外景 | 195 KB |
| `孙伯伯家里屋.jpg` | 孙伯伯家里屋内景 | 180 KB |
| `河边树林外.png` | 池塘边树林场景 | 3.2 MB |
| `none` | 不显示背景（纯色或渐变） | - |

#### 特殊值

- `none`: 移除背景图片
- 未指定: 保持上一个背景

#### 示例

```ink
=== act1_after_school ===
# bg: 放学路_第一幕主背景.jpg
放学铃声响起......

=== act2A_sun_house ===
# bg: 孙伯伯家院子.jpg
你跟着孙伯伯走进他家的院子......

=== prologue ===
# bg: none
你好，欢迎来到这个互动故事......
```

---

### 2. 角色图片 (Characters)

#### 2.1 左侧角色 (char_left)

**标签名**: `char_left`
**功能**: 在画面左侧显示角色

**语法**:
```ink
# char_left: 文件名.png
```

**可用值**:

| 文件名 | 角色/状态 | 尺寸 |
|--------|----------|------|
| `小雨平静版.png` | 主角-平静状态 | 2.5 MB |
| `小雨害怕版1.png` | 主角-警觉/害怕 | 2.7 MB |
| `小雨害怕版2.png` | 主角-严重恐惧 | 2.7 MB |
| `none` | 移除左侧角色 | - |

#### 2.2 右侧角色 (char_right)

**标签名**: `char_right`
**功能**: 在画面右侧显示角色

**可用值**:

| 文件名 | 角色/状态 | 尺寸 |
|--------|----------|------|
| `孙伯伯和蔼版.png` | 孙伯伯-伪装友好 | 1.9 MB |
| `孙伯伯阴沉版.png` | 孙伯伯-露出真面目 | 2.3 MB |
| `女警.png` | 警察阿姨 | 1.7 MB |
| `none` | 移除右侧角色 | - |

#### 2.3 中央角色 (char_center)

**标签名**: `char_center`
**功能**: 在画面中央显示单个角色（用于独白、特写场景）

**可用值**: 同左侧角色

#### 角色位置规则

- **双人对话**: 使用 `char_left` + `char_right`
- **独白/结局**: 使用 `char_center`
- **三人场景**: 使用 `char_left` + `char_center` + `char_right` (慎用)

#### 示例

```ink
=== act1_after_school ===
# bg: 放学路_第一幕主背景.jpg
# char_left: 小雨平静版.png
# char_right: 孙伯伯和蔼版.png

孙伯伯站在他家门口，笑容满面地向你招手......

=== assault_scene ===
# bg: 孙伯伯家里屋.jpg
# bg_filter: darken
# char_center: 小雨害怕版2.png
# char_left: none
# char_right: none

你蜷缩在沙发角落，浑身发抖......
```

---

### 3. 背景滤镜 (bg_filter)

**标签名**: `bg_filter`
**功能**: 对背景图应用视觉滤镜

#### 可用值

| 滤镜名 | 效果 | 用途 |
|--------|------|------|
| `darken` | 背景变暗 (opacity 0.5) | 危险/恐怖场景 |
| `brighten` | 背景变亮 (brightness 1.3) | 正面结局 |
| `grayscale` | 黑白滤镜 | 悲伤结局 |
| `blur` | 模糊背景 (blur 5px) | 强调前景角色 |
| `none` | 移除滤镜 | - |

#### 语法

```ink
# bg_filter: darken
```

#### 示例

```ink
=== assault_scene ===
# bg: 孙伯伯家里屋.jpg
# bg_filter: darken
# char_center: 小雨害怕版2.png

里屋的门关上了......

=== ending_D_severe ===
# bg: 河边树林外.png
# bg_filter: grayscale
# char_center: 小雨害怕版2.png

多年以后......
```

---

### 4. 情绪氛围 (mood)

**标签名**: `mood`
**功能**: 设置场景情绪，用于CSS样式切换（边框颜色、字体颜色等）

#### 可用值

| 情绪值 | 含义 | CSS效果 |
|--------|------|---------|
| `calm` | 平静 | 蓝色边框 |
| `safe` | 安全 | 绿色边框 |
| `tense` | 紧张 | 黄色边框 |
| `danger` | 危险 | 红色边框 |
| `sad` | 悲伤 | 灰色边框 |
| `happy` | 快乐 | 亮绿色边框 |

#### 语法

```ink
# mood: danger
```

#### 示例

```ink
=== act1_after_school ===
# bg: 放学路_第一幕主背景.jpg
# mood: calm

放学铃声响起......

=== act3A_danger_path ===
# bg: 孙伯伯家里屋.jpg
# mood: danger

里屋的门关上了......
```

---

### 5. 特效 (effect)

**标签名**: `effect`
**功能**: 触发一次性视觉特效

#### 可用值

| 特效名 | 效果 | 用途 |
|--------|------|------|
| `shake` | 画面抖动 (0.5秒) | 惊吓、冲击 |
| `fade` | 淡入淡出 (1秒) | 场景过渡 |
| `flash` | 闪光 (0.3秒) | 惊恐瞬间 |
| `none` | 无特效 | - |

#### 语法

```ink
# effect: shake
```

#### 示例

```ink
=== second_assault_occurs ===
# bg: 河边树林外.png
# effect: shake

突然！一只有力的大手从背后抓住了你！
```

---

## 四、Tags组合模式

### 模式1: 完整场景设置 (Scene Setup)

用于knot开头，设置完整场景

```ink
=== act3A_danger_path ===
# bg: 孙伯伯家里屋.jpg
# char_left: 小雨害怕版1.png
# char_right: 孙伯伯阴沉版.png
# mood: danger

里屋的门关上了......
```

### 模式2: 角色状态切换 (Character Change)

场景中角色表情/状态变化

```ink
"真的不进里屋吗？那边有空调，很凉快。"他再次劝说。 # char_right: 孙伯伯阴沉版.png

气氛变得越来越尴尬，你开始感到有些不对劲...... # char_left: 小雨害怕版1.png
```

### 模式3: 背景过渡 (Background Transition)

切换场景背景

```ink
=== escape_reflect ===
# bg: 放学路_第一幕主背景.jpg
# char_center: 小雨害怕版1.png
# mood: safe

你成功离开了危险的环境......
```

### 模式4: 特效增强 (Effect Enhancement)

添加特效强化氛围

```ink
突然！一只有力的大手从背后抓住了你！ # effect: shake # char_right: 孙伯伯阴沉版.png
```

### 模式5: 滤镜叠加 (Filter Overlay)

背景滤镜 + 角色调整

```ink
=== assault_scene ===
# bg: 孙伯伯家里屋.jpg
# bg_filter: darken
# char_center: 小雨害怕版2.png
# char_left: none
# char_right: none
# mood: danger

你蜷缩在沙发角落......
```

---

## 五、实施指南

### Step 1: 识别场景类型

对于每个knot/stitch，确定:
1. 需要什么背景？
2. 有几个角色？分别是谁？
3. 情绪氛围是什么？
4. 需要特效吗？

### Step 2: 选择合适的Tags

参考"三、标准Tags定义"选择适当的tag组合

### Step 3: 在脚本中添加Tags

**推荐位置**: knot/stitch开头（在第一行内容之前）

```ink
=== your_knot ===
# bg: 文件名.jpg
# char_left: 文件名.png
# char_right: 文件名.png
# mood: 情绪值

第一行内容......
```

### Step 4: 测试

在Inky编辑器中测试脚本逻辑（tags不影响逻辑）
导出Web版本，在浏览器中测试图片显示

---

## 六、最佳实践

### ✅ DO (推荐做法)

1. **在knot开头集中定义场景tags**
   ```ink
   === knot_name ===
   # bg: xxx.jpg
   # char_left: xxx.png
   # char_right: xxx.png
   ```

2. **角色状态变化时立即更新tag**
   ```ink
   孙伯伯的表情变了变。 # char_right: 孙伯伯阴沉版.png
   ```

3. **使用none移除不需要的角色**
   ```ink
   # char_left: none
   # char_right: none
   # char_center: 小雨害怕版2.png
   ```

4. **保持tag命名一致性**
   - 始终使用同样的文件名（包括扩展名）
   - 文件名区分大小写

5. **添加注释说明特殊tag用途**
   ```ink
   # bg: 孙伯伯家里屋.jpg
   # bg_filter: darken  // 营造恐怖氛围
   ```

### ❌ DON'T (避免做法)

1. **不要在tag中使用中文标点**
   ```ink
   ❌ # bg：文件名.jpg    (使用了中文冒号)
   ✅ # bg: 文件名.jpg
   ```

2. **不要忘记文件扩展名**
   ```ink
   ❌ # bg: 放学路_第一幕主背景
   ✅ # bg: 放学路_第一幕主背景.jpg
   ```

3. **不要使用不存在的文件名**
   - 确保文件在`graphics/`文件夹中

4. **不要混合不同的命名风格**
   ```ink
   ❌ # background: xxx.jpg
   ❌ # Background: xxx.jpg
   ✅ # bg: xxx.jpg
   ```

5. **不要在一行中定义矛盾的tags**
   ```ink
   ❌ # char_center: 小雨.png # char_left: 小雨.png  (同时中央和左侧)
   ```

---

## 七、Tags优先级规则

当多个tags冲突时的处理规则:

### 1. 角色位置冲突

如果同时定义了`char_center`和`char_left/char_right`:
- 优先显示`char_center`
- 隐藏`char_left`和`char_right`

### 2. 背景滤镜叠加

多个滤镜同时存在:
- 只应用最后一个滤镜
- 建议: 一次只使用一个滤镜

### 3. 情绪冲突

- 只应用最后一个`mood`值

### 4. 特效叠加

- 所有`effect`都会依次触发
- 建议: 一次只使用一个特效

---

## 八、示例：完整场景标记

### 示例1: 第一幕 - 放学路上

```ink
=== act1_after_school ===
~ current_act = 1

# bg: 放学路_第一幕主背景.jpg
# char_left: 小雨平静版.png
# char_right: 孙伯伯和蔼版.png
# mood: calm

放学铃声响起，同学们陆续离开教室。

你背着书包走在回家的乡村小路上，天色渐晚，夕阳把你的影子拉得很长。

前方路口，你看到两个熟悉的身影：

孙伯伯站在他家门口，笑容满面地向你招手："小雨啊，我小孙子一直找你玩呢，要不要去我家玩会儿？"

邻居家的阿强哥哥骑着自行车停在路边："嘿，我家刚下载了新游戏，特别好玩，要不要来看看？"

孙伯伯和阿强哥哥都在等你的回答。

你想：奶奶说晚饭快好了，但去玩一会儿应该也没关系吧？

你决定：

+ [去孙伯伯家看看小孙子]
    你想：反正就在隔壁，玩一会儿就回家，应该没问题。
    -> act2A_sun_house
```

### 示例2: 第三幕 - 危险升级

```ink
=== act3A_stage2 ===
# bg: 孙伯伯家里屋.jpg
# char_left: 小雨害怕版1.png
# char_right: 孙伯伯阴沉版.png
# mood: tense

孙伯伯没有理会你的话，反而坐得更近了。

他伸手拍了拍你的肩膀，手在你肩膀上停留了好几秒。

"你学习累不累？孙伯伯给你按按肩膀吧。"

你的身体本能地感到不舒服。 # effect: shake

💡 提示：任何让你不舒服的身体接触，你都有权拒绝。先离开现场，再尽快告诉大人。

📖 法律小课堂：法律保护你的身体自主权。
• 任何人不得实施猥亵、骚扰或其他不当身体接触
• 即使对方是熟人、邻居或长辈，也不例外

+ [躲开他的手，站起来说"我真的要走了"]
    # char_left: 小雨害怕版2.png
    你想：我得离开这里。
    你站起身，避开他的手："我真的要回家了。"
    -> act3A_try_leave
```

### 示例3: 侵害场景（敏感场景）

```ink
=== assault_scene ===
~ assault_occurred = true

# bg: 孙伯伯家里屋.jpg
# bg_filter: darken
# char_center: 小雨害怕版2.png
# char_left: none
# char_right: none
# mood: danger
# effect: fade

你试图挣扎，但孙伯伯用力抓住你的手臂，把你按在沙发上。

他开始扯你的衣服。你拼命想推开他，但一个孩子的力气根本不可能推开一个成年男人。

[内容省略]

不知道过了多久，终于结束了。

你蜷缩在沙发角落，浑身发抖，眼泪止不住地流。

[后续内容...]

-> act4_silence_path
```

### 示例4: 警察调查（情绪转折）

```ink
=== police_investigation ===
# bg: none
# char_left: 小雨害怕版1.png
# char_right: 女警.png
# mood: safe

警察很快来到了你家。

穿着制服的女警察蹲下来，温柔地对你说："小朋友，你很勇敢。现在，你能再告诉我一次发生了什么吗？"

你把事情从头到尾说了一遍。

警察认真记录，然后对你的父母说："孩子的陈述非常清晰，我们会立即调查。现在先带孩子去医院做检查，保留证据很重要。"

女警察还告诉你："小朋友，这不是你的错。你做得对，报警能让坏人受到惩罚。" # char_left: 小雨平静版.png

[后续内容...]
```

---

## 九、Web播放器实现（开发者参考）

### JavaScript Tags读取示例

```javascript
function continueStory() {
    if (!story.canContinue) return;

    // 获取下一段内容
    var paragraphText = story.Continue();

    // 读取当前tags
    var currentTags = story.currentTags;

    // 处理tags
    processTags(currentTags);

    // 显示文本
    displayText(paragraphText);
}

function processTags(tags) {
    tags.forEach(function(tag) {
        var parts = tag.split(':').map(s => s.trim());
        var tagName = parts[0];
        var tagValue = parts[1];

        switch(tagName) {
            case 'bg':
                updateBackground(tagValue);
                break;
            case 'char_left':
                updateCharacter('left', tagValue);
                break;
            case 'char_right':
                updateCharacter('right', tagValue);
                break;
            case 'char_center':
                updateCharacter('center', tagValue);
                break;
            case 'mood':
                updateMood(tagValue);
                break;
            case 'effect':
                triggerEffect(tagValue);
                break;
            case 'bg_filter':
                applyBackgroundFilter(tagValue);
                break;
        }
    });
}

function updateBackground(filename) {
    if (filename === 'none') {
        document.getElementById('background').style.display = 'none';
    } else {
        var bg = document.getElementById('background');
        bg.src = 'graphics/background/' + filename;
        bg.style.display = 'block';
    }
}

function updateCharacter(position, filename) {
    var elementId = 'char-' + position;
    var element = document.getElementById(elementId);

    if (filename === 'none') {
        element.style.display = 'none';
    } else {
        element.src = 'graphics/characters/' + filename;
        element.style.display = 'block';
    }
}
```

---

## 十、快速参考

### Tags速查表

| Tag | 用途 | 示例 |
|-----|------|------|
| `bg` | 背景图 | `# bg: 放学路_第一幕主背景.jpg` |
| `char_left` | 左侧角色 | `# char_left: 小雨平静版.png` |
| `char_right` | 右侧角色 | `# char_right: 孙伯伯和蔼版.png` |
| `char_center` | 中央角色 | `# char_center: 小雨害怕版2.png` |
| `mood` | 情绪氛围 | `# mood: danger` |
| `effect` | 特效 | `# effect: shake` |
| `bg_filter` | 背景滤镜 | `# bg_filter: darken` |

### 常用组合模式

**双人对话场景**:
```ink
# bg: 背景.jpg
# char_left: 角色1.png
# char_right: 角色2.png
```

**独白/特写**:
```ink
# bg: 背景.jpg
# char_center: 角色.png
# char_left: none
# char_right: none
```

**危险场景**:
```ink
# bg: 背景.jpg
# bg_filter: darken
# mood: danger
# effect: shake
```

---

## 十一、版本历史

| 版本 | 日期 | 变更内容 |
|------|------|----------|
| 1.0 | 2026-04-02 | 初始版本，定义所有标准tags |

---

## 十二、FAQ

**Q: Tags会显示给玩家吗？**
A: 不会。Tags是隐藏的元数据，只有游戏引擎能读取。

**Q: 在Inky编辑器中能看到图片吗？**
A: 不能。必须导出Web版本才能看到图片效果。

**Q: Tags大小写敏感吗？**
A: 标签名不敏感（`bg`和`BG`相同），但文件名敏感（`文件.jpg`和`文件.JPG`不同）。

**Q: 可以使用中文文件名吗？**
A: 可以，但建议避免，可能在某些服务器上有兼容性问题。

**Q: Tags的数量有限制吗？**
A: 没有硬性限制，但建议一行不超过5-6个tags，保持可读性。

**Q: 可以自定义新的tag吗？**
A: 可以，但需要在Web播放器中添加对应的处理代码。

---

**文档结束**

**下一步**: 在v1.3脚本中添加tags →
