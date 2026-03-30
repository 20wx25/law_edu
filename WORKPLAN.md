# 普法情景游戏 - Ink Implementation Work Plan

**Game Title**: 普法情景游戏 (Legal Education Interactive Game)
**Target Audience**: Students (learning about 防性侵法律知识和求救常识)
**Engine**: Ink Script Language
**Status**: Foundation Phase

---

## Feasibility Assessment

### ✅ HIGHLY FEASIBLE

Your game structure is excellently suited for Ink implementation.

#### What Works Perfectly in Ink:

1. **✅ Three-Variable System**
   - Ink supports variables with `VAR Risk = 0`
   - Can hide Risk from player display (implementation-dependent)
   - Easy arithmetic operations: `~ Risk += 2`

2. **✅ Threshold-Triggered Branching**
   ```ink
   {Risk >= 5:
       NPC: "进屋说吧,外面冷。"
   - else:
       NPC: "就在这儿说吧。"
   }
   ```

3. **✅ Complex Conditional Logic**
   - Supports multi-variable conditions: `{Risk >= 7 and Support <= 2: ...}`
   - Perfect for variable interdependencies

4. **✅ Branching Structure**
   - Knots = Acts/Scenes
   - Choices naturally map to A/B/C/D options
   - Diverts handle path navigation

5. **✅ Multiple Endings**
   - Conditional diverts to reach different ending knots
   - Variable-based ending selection works perfectly

#### Design Adaptations Needed:

1. **⚠️ "弹窗提示" (Popup Notifications)**
   - **Solution**: Use formatted text blocks with clear visual separators
   ```ink
   === === 【安全提示】 === ===
   尽量避免单独进入他人家中
   === === === === === ===
   ```

2. **⚠️ "复盘模块" (Recap System)**
   - **Solution**: Create reusable tunnel functions
   - Can show variable changes after each choice

3. **⚠️ Variable Display (Harm/Support visible)**
   - **Solution**: Print at key moments or use functions to display status
   ```ink
   === function show_status() ===
   [当前状态] 伤害值: {Harm} | 支持值: {Support}
   ```

---

## Core Game Mechanics

### Three-Variable System

| 变量 | 含义 | 玩家可见 | 作用 |
|------|------|----------|------|
| Risk（风险值） | 当前环境危险程度 | ❌不可见 | 决定剧情是否升级 |
| Harm（伤害值） | 已发生心理/身体伤害累计 | ✅可见 | 决定创伤程度 |
| Support（支持值） | 外界帮助资源水平 | ✅可见 | 决定是否进入保护路径 |

### Risk (风险值) Mechanics

#### Risk增加机制
| 行为 | Risk变化 |
|------|----------|
| 单独跟成年人走 | +2 |
| 进入封闭空间 | +2 |
| 进入对方家里无人环境 | +2 |
| 隐瞒去向 | +1 |
| 继续接触危险人物 | +2 |
| 夜晚单独行动 | +2 |

#### Risk阈值触发机制
| Risk区间 | 触发剧情变化 |
|----------|--------------|
| 0–2 | 安全环境 |
| 3–4 | NPC行为异常 |
| 5–6 | NPC主动制造独处机会 |
| 7–8 | 强制侵害节点出现 |
| 9+ | 高概率既遂侵害 |

**示例**:
- Risk≥5时: NPC说："进屋说吧,外面冷。"
- Risk≥7: NPC直接靠近身体距离

### Harm (伤害值) Mechanics

**核心教育变量** - 表示：心理创伤 + 身体伤害 + 持续风险累积

#### Harm增加机制
| 行为 | Harm变化 |
|------|----------|
| 进入危险环境未离开 | +1 |
| 被威胁后沉默 | +2 |
| 再次单独接触施害者 | +2 |
| 发生身体侵害 | +5 |
| 持续沉默超过一幕 | +3 |

#### Harm阈值触发机制
| Harm区间 | 触发效果 |
|----------|----------|
| 0–2 | 警觉提示 |
| 3–5 | 情绪异常剧情 |
| 6–8 | 社交退缩剧情 |
| 9–12 | PTSD剧情线 |
| 13+ | 严重负面结局线 |

**示例**: Harm≥6时新增剧情：
- 夜晚做噩梦
- 课堂走神
- 不敢独处

#### Harm下降机制
| 行为 | Harm变化 |
|------|----------|
| 告诉家人 | -3 |
| 告诉老师 | -2 |
| 报警 | -5 |
| 远离施害者 | -1 |
| 心理辅导 | -2 |

**关键原则**: 报警必须是最大恢复路径 (否则教育方向错误)

### Support (支持值) Mechanics

Support决定玩家是否进入保护系统

#### Support增加机制
| 行为 | Support变化 |
|------|-------------|
| 告诉家人 | +2 |
| 告诉老师 | +2 |
| 报警 | +3 |
| 联系妇联12338 | +3 |
| 朋友陪伴 | +1 |

#### Support阈值触发机制
| Support区间 | 开启剧情 |
|-------------|----------|
| 0 | 无保护路径 |
| 2 | 家庭保护路径 |
| 4 | 学校保护路径 |
| 6 | 警方介入路径 |
| 8+ | 心理恢复路径 |

### Variable Interdependency Logic

**真实运行逻辑**:
- Risk决定是否发生危险事件
- Harm决定伤害程度
- Support决定是否脱离危险

**示例**:
```
Risk≥7 且 Support≤2 → 强制侵害剧情
Risk≥7 且 Support≥4 → NPC中断行为（被外界干预）
```

---

## Story Structure

### Complete Act Flow

1. **第一幕：放学路上**
   - 场景：乡村小路
   - NPC：孙伯伯、阿强
   - 选择：去孙伯伯家/去阿强家/直接回家/等同学
   - 教育：【安全提示】避免单独进入他人家中

2. **第二幕A：孙伯伯家**
   - 场景：堂屋 → 里屋选择
   - NPC靠近、制造独处机会
   - 教育：【识别风险提示】成年人主动制造独处环境时要提高警惕

3. **第二幕B：阿强家路径**
   - 场景：房间较暗，桌上有刀
   - 逃生选择
   - 教育：【逃生提示】制造声音可以阻止侵害

4. **第三幕A：危险路径**
   - Risk≥5触发身体靠近
   - 立即离开 vs 顺从选择
   - 教育：【自救提示】遇到危险要立即离开现场

5. **第四幕A：沉默路径**
   - NPC威胁："敢说出去就没人相信你"
   - 求助选择：告诉奶奶/老师/报警/沉默
   - 教育：【法律提示】威胁不能阻止犯罪成立

6. **第四幕B：报警路径**
   - 场景：派出所
   - 警察阿姨对话
   - 教育：【证据提示】不要洗衣服、尽快就医

7. **第五幕：长期沉默路径**
   - 连续沉默触发：注意力下降 → 不愿上学 → 身体异常
   - 教育：【长期风险提示】沉默可能导致持续侵害

8. **复盘模块**（每幕结束）
   - 显示：Risk/Harm/Support变化
   - 提示：更优选择路径

### Four Endings System

| 结局 | 触发条件 | 结果 |
|------|----------|------|
| **结局A（最佳）** | Support≥6 且 Harm≤4 | 施害者被判刑，恢复上学 |
| **结局B（中等）** | Support≥4 且 Harm≤7 | 侵害停止，心理恢复中 |
| **结局C（危险）** | Support≤2 且 Harm≥8 | 长期创伤 |
| **结局D（严重）** | Support≤1 且 Harm≥12 | PTSD / 辍学路径 |

---

## Development Phases

### Phase 1: Foundation Setup (Core Systems) ⬅️ CURRENT PHASE
**Deliverable**: Working variable system and helper functions

**Tasks**:
- [x] Initialize three core variables (Risk, Harm, Support)
- [x] Create utility functions:
  - `status_display()` - Show Harm/Support to player
  - `safety_tip(message)` - Display educational popups
  - `recap(risk_change, harm_change, support_change)` - Show choice consequences
  - `check_ending()` - Determine ending based on final variables
- [x] Create test scaffold with initial divert
- [ ] Test in Inky to verify mechanics work

**File**: `普法情景游戏.ink` (foundation)

---

### Phase 2: Act Structure (Story Skeleton)
**Deliverable**: Complete knot structure with placeholder content

**Tasks**:
1. Create main act knots:
   - `=== prologue ===` - Game introduction
   - `=== act1_after_school ===` - 放学路上
   - `=== act2A_sun_house ===` - 孙伯伯家路径
   - `=== act2B_aqiang_house ===` - 阿强家路径
   - `=== act3A_danger_path ===` - 危险升级
   - `=== act3B_aqiang_danger ===` - 阿强危险路径
   - `=== act4_silence_path ===` - 沉默路径
   - `=== act4_police_path ===` - 报警路径
   - `=== act5_longterm_silence ===` - 长期沉默
   - `=== endings ===` - Ending dispatcher

2. Create ending knots:
   - `=== ending_A_best ===`
   - `=== ending_B_medium ===`
   - `=== ending_C_danger ===`
   - `=== ending_D_severe ===`

3. Map divert flow to ensure all paths reach endings

---

### Phase 3: Act 1 Implementation (First Playable)
**Deliverable**: Fully playable Act 1 with all mechanics working

**Tasks**:
1. Write Act 1 dialogue (opening scene, NPC dialogue, player choices)
2. Implement variable changes for each choice
3. Add safety tips after risky choices
4. Add recap module showing variable changes
5. Test threshold triggers (Risk >= 3 for NPC behavior changes)

---

### Phase 4: Act 2-3 Implementation (Core Danger Paths)
**Deliverable**: Complete danger escalation mechanics

**Tasks**:
1. Implement Act 2A (孙伯伯家): 堂屋/里屋 scenes
2. Implement Act 2B (阿强家): 房间场景 + escape choices
3. Implement Act 3 danger paths with physical approach scenes
4. Add conditional branching based on Risk/Support thresholds
5. Test Harm threshold triggers (Harm 3-5, 6-8, 9-12 effects)

---

### Phase 5: Act 4-5 Implementation (Response Paths)
**Deliverable**: Complete help-seeking and silence consequences

**Tasks**:
1. Implement reporting paths (告诉奶奶/老师/报警/妇联)
2. Implement silence consequences (Stage 1-3)
3. Write police station scene
4. Add psychological trauma scenes (Harm >= 6)

---

### Phase 6: Endings Implementation
**Deliverable**: Four complete endings with educational messages

**Tasks**:
1. Implement ending dispatcher with conditional logic
2. Write four ending narratives (A/B/C/D)
3. Add final educational summary for each ending
4. Add replay option

---

### Phase 7: Polish & Testing
**Deliverable**: Production-ready game

**Tasks**:
1. Educational content review (verify legal accuracy)
2. Playtest all paths (best/worst/middle paths)
3. Balance variables (ensure all endings are reachable)
4. Add Chinese text formatting
5. Create README.md

---

### Phase 8: Documentation & Handoff
**Deliverable**: Complete documentation for future modifications

**Tasks**:
1. Create game design document
2. Write modifier guide (how to add scenes/adjust thresholds)
3. Create testing checklist

---

## Recommended Timeline

- **Week 1**: Phases 1-2 (Foundation + Structure)
- **Week 2**: Phase 3 (Act 1 fully working)
- **Week 3**: Phases 4-5 (Acts 2-5 implementation)
- **Week 4**: Phases 6-8 (Endings + Polish + Documentation)

---

## File Structure

```
/inky/
├── 普法情景游戏.ink         # Main game file
├── WORKPLAN.md              # This file - development roadmap
├── README.md                # Game instructions (Phase 7)
├── DESIGN.md                # Variable mechanics documentation (Phase 8)
└── TESTING.md               # Playtest checklist (Phase 8)
```

---

## Technical Notes

### Ink Syntax Reminders

**Variables**:
```ink
VAR Risk = 0
~ Risk += 2
~ Risk = 0  // Reset
```

**Conditionals**:
```ink
{Risk >= 5:
    This happens if Risk is 5 or more
- else:
    This happens otherwise
}

{Risk >= 7 and Support <= 2:
    Dangerous situation with no help
}
```

**Functions** (for reusable content):
```ink
=== function safety_tip(message) ===
=== === 【安全提示】 === ===
{message}
=== === === === === ===
~return
```

**Tunnels** (for sequences that return):
```ink
-> recap(2, 1, 0) ->
// Shows recap, then continues
```

**Diverts** (navigation):
```ink
-> act2A_sun_house  // Go to knot
-> END              // End story
```

---

## Key Educational Principles

1. **"报警" must always provide the best outcome** (largest Support increase, largest Harm decrease)
2. **Risk is hidden** to simulate real-world ambiguity (players don't always know danger level)
3. **Harm and Support are visible** to help players understand consequences
4. **Popups appear at teaching moments** (after risky choices)
5. **Recap shows why choices matter** (immediate feedback)
6. **Multiple paths lead to safety** (not just one "correct" playthrough)
7. **Silence always worsens situation** (clear educational message)

---

## Next Steps

1. ✅ Complete Phase 1 foundation implementation
2. Test in Inky to verify variables and functions work
3. Proceed to Phase 2 (story structure)

---

**Document Version**: 1.0
**Last Updated**: 2024-03-28
**Status**: Phase 1 in progress
