# Quick Testing Setup 🚀

## ✅ Files Ready
- ✅ `web_player.html` - Web player
- ✅ `ink.js` - Runtime library (123KB)
- ✅ `graphics/` folder with all images
- ✅ `普法情景游戏_v1.4_with_tags.ink` - Tagged script

## ⏳ Next Step: Export story.json

### Using Inky Editor

1. **Open Inky**
2. **File** → **Export** → **Export story.js for web...**
3. **Save as**: `story.json`
4. **Location**: `/Users/liyuelin/Desktop/Claude/inky/` (same folder as web_player.html)

## 🚀 Run the Game

### Start a local server:

```bash
cd /Users/liyuelin/Desktop/Claude/inky
python3 -m http.server 8000
```

### Open in browser:

```
http://localhost:8000/web_player.html
```

## 🎮 What You Should See

**Scene 1** - 放学后:
- Background: 放学路_第一幕主背景.jpg
- Left: 小雨平静版.png
- Right: 孙伯伯和蔼版.png
- Blue border (calm mood)

**Scene 2** - 孙伯伯家:
- Background switches to 院子.jpg
- Characters remain visible

**Danger Scene**:
- Background: 里屋.jpg
- Characters change to 害怕版/阴沉版
- Red border (danger mood)
- Screen shake effect

## 🐛 Troubleshooting

**Images not loading?**
- Check browser Console (F12)
- Verify graphics folder structure
- Ensure file names match exactly (case-sensitive)

**story.json error?**
- Make sure you exported from Inky
- File should be in the same folder as web_player.html

**Still shows "正在加载游戏..."?**
- Check if story.json exists: `ls -lh story.json`
- Check browser Console for errors

---

📖 **Full guide**: See `TESTING_GUIDE.md` for comprehensive testing checklist
