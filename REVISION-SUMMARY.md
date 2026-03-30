# 普法情景游戏 - Revision Summary
**Date**: 2024-03-28
**Focus**: Realism, Information Sets, and Gradual Alertness Building

---

## Core Principles Implemented

### 1. **Respect Player's Information Set**
- Choices now reflect what the player character **actually knows** at each moment
- No premature suspicion or alertness without narrative justification

### 2. **Gradual Alertness Escalation**
- **Stage 1**: No reason to be suspicious (normal invitation from familiar neighbor)
- **Stage 2**: Slight unease (invited to unfamiliar inner room)
- **Stage 3**: Growing concern (insisting after refusal, awkward atmosphere)
- **Stage 4**: Clear danger (door closed, approaching, strange behavior)

### 3. **Psychological Rationalization (心理活动)**
- Every choice now includes "你想：..." to explain player's thinking
- Natural decision-making process, not "safety quiz" responses

### 4. **Less Preachy, More Immersive**
- Removed many explicit educational popups
- Integrated learning into natural narrative flow
- Educational moments come through experience, not lectures

---

## Major Changes

### Act 1: Initial Invitation (放学路上)

**REMOVED** ❌:
- "直接回家" option (no reason to refuse yet)
- "等其他同学一起走" option (no reason to be cautious yet)

**KEPT** ✅:
- "去孙伯伯家看看小孙子" (with psychological reasoning)
- "跟阿强哥哥去看新游戏" (with psychological reasoning)

**Added心理活动**:
```ink
你想：反正就在隔壁，玩一会儿就回家，应该没问题。
你想：新游戏听起来挺有意思的，去看看也不错。
```

**Rationale**: At this point, player has ZERO information suggesting danger. A normal student would make mundane choices based on interest, not safety concerns.

---

### Act 2A Initial: First Invitation to Inner Room

**Context**: Player feels "有些不安" (slightly uneasy) because never been in inner room before.

**REMOVED** ❌:
- "找借口：'我想起奶奶让我买东西，我得先去小卖部'" (TOO ALERT for this stage)
- "给奶奶打电话：'奶奶，我在孙伯伯家'" (TOO ALERT for this stage)

**KEPT** ✅:
- "还是留在堂屋好了" (polite refusal with reasoning)
- "进去坐坐吧，孙伯伯应该是好意" (compliance with reasoning)

**Added心理活动**:
```ink
你想：里屋我没去过，还是在堂屋比较自在。
你想：孙伯伯应该是好客，进去坐坐应该没事。
```

**Rationale**: Slight unease → polite refusal OR compliance. NOT yet at "make excuses to flee" level.

---

### Act 2A Stage 2: After Refusing Once (hall_stay)

**Context**: Player refused once, 孙伯伯 insists, "气氛变得越来越尴尬，你开始感到有些不对劲......"

**ADDED** ✅ (moved from previous stage):
- "找个借口离开：'我忘了奶奶让我买东西'"
- "拿出手机给奶奶打电话"

**ALL choices now have psychological reasoning**:
```ink
你想：气氛有点奇怪，还是回家吧。
你想：感觉有点不对，找个理由离开吧。
你想：给奶奶打个电话，让她知道我在哪里。
你想：可能是自己多想了，再待一会儿吧。
```

**Rationale**: NOW the alertness level justifies protective actions. Player has updated information (insisting, staring, awkward) that triggers concern.

---

### Act 3A: Danger Escalation

**Changes**:
- Added emotional context: "你心里一紧——这个情况让你很不安。"
- All choices now include心理活动:
  - "你的直觉告诉你要赶快离开！"
  - "你想：大声说话，邻居可能会听到。"
  - "你的脑子一片空白，不知道该做什么......"

- **Removed preachy tips**:
  - ❌ "遇到危险时，立即逃离是最重要的。不要犹豫！"
  - ❌ "制造声音可以吓退施害者，也能引来帮助。"
  - ❌ "顺从不会让危险消失。记住：你有权利说不，有权利反抗。"

**Rationale**: At danger stage, choices should reflect panic/instinct, not educational lectures. Learning happens through experiencing consequences, not being told.

---

### Moderate Danger Section

**Added psychological context**:
```ink
他伸手想碰你的肩膀......你本能地感到不舒服。

你的身体本能地往后退，朝门口移动。
你鼓起勇气大声说出来。
你不知道该怎么办，愣住了......
```

**Rationale**: Emphasizes instinctive, emotional responses rather than calculated safety decisions.

---

### Act 2A Call Made Section

**REMOVED** ❌:
```ink
-> safety_tip("注意到了吗？当你让家人知道你的位置时，危险的人往往会打消念头。这就是为什么告知行踪很重要。")
```

**REPLACED WITH** ✅:
```ink
不过，能离开这里你还是松了一口气。
```

**Rationale**: Natural relief emotion instead of meta-commentary on safety strategy.

---

## Educational Content Strategy

### Before (Explicit):
- Safety tips after every choice
- Educational popups explaining "why this matters"
- Meta-commentary interrupting narrative

### After (Integrated):
- Learning through experiencing consequences
- Educational moments at natural reflection points (escape_reflect, endings)
- Player infers lessons from story outcomes

### Kept Educational Content:
- **escape_reflect**: Still has educational popup (justified - player is reflecting on what happened)
- **Endings**: Educational summaries (justified - post-experience learning)
- **Recap system**: Shows variable changes (helps understanding consequences)

---

## Information Set Progression (Summary)

| Stage | Information | Feeling | Appropriate Actions |
|-------|-------------|---------|---------------------|
| **Act 1** | Normal invitation | 无 (neutral) | Go with A/B based on interest |
| **Act 2A Initial** | Invitation to unfamiliar room | 有些不安 (slightly uneasy) | Polite refusal OR compliance |
| **Act 2A Stage 2** | Insisting after refusal, staring | 开始感到有些不对劲 (feeling something wrong) | Make excuses, call family, leave |
| **Act 3A** | Door closed, approaching | 很不安 (very uneasy/panicked) | Run, shout, freeze |

---

## Testing Checklist

Please playtest these paths to verify the changes feel natural:

### Path 1: Gradual Escalation
1. Act 1: Choose "去孙伯伯家看看小孙子"
2. Act 2A: Choose "还是留在堂屋好了" (refuse inner room)
3. Stage 2: Choose "说'我该回家了'，站起身准备离开"
4. Check: Does the escalation feel natural? Does each choice match the player's knowledge at that moment?

### Path 2: Naive Compliance
1. Act 1: Choose "去孙伯伯家看看小孙子"
2. Act 2A: Choose "进去坐坐吧，孙伯伯应该是好意"
3. Act 3A: [Danger choices]
4. Check: Does the psychology make sense? "孙伯伯应该是好意" reflects innocent thinking?

### Path 3: Growing Awareness
1. Act 1: Choose "去孙伯伯家看看小孙子"
2. Act 2A: Choose "还是留在堂屋好了"
3. Stage 2: Choose "拿出手机给奶奶打电话"
4. Check: Does calling family make sense at THIS point (after seeing weird behavior) but not earlier?

---

## Remaining Work

### Completed ✅:
- Remove premature alert options from Act 1
- Revise Act 2A initial choices
- Move protective options to appropriate stage
- Add psychological reasoning throughout
- Soften preachy educational content in main narrative

### Not Yet Done:
- Act 2B (阿强路径) - user wants to postpone this
- Act 4-5 implementation
- Endings B/C/D (only Ending A is complete)

---

## Key Learning for Future Writing

1. **Always ask**: "What does the player character KNOW at this moment?"
2. **Escalation must be justified**: Each increase in alertness needs narrative trigger
3. **心理活动 is critical**: "你想：..." makes choices feel real
4. **Less is more**: Educational content through experience > explicit tips
5. **Respect realistic psychology**: Players don't become safety experts instantly

---

## User Feedback Implemented

✅ Removed "直接回家" - no reason to refuse at start
✅ Removed/rationalized "等其他同学一起走" - no reason to be cautious yet
✅ Moved "找借口" and "给奶奶打电话" to later stage
✅ Added rationalization throughout (你从未进过孙伯伯家的里屋，因此听到这样的邀请感到有些不安......)
✅ Made game feel like "real-life experience" not "educational quiz"
✅ Respected information sets and alertness levels

---

**Next Steps**: Please playtest and provide feedback on realism and pacing!
