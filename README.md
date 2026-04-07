# 守护少年：普法情景游戏

**Legal Education Interactive Game for Children**

An interactive fiction game built with [Ink](https://www.inklestudios.com/ink/) that teaches children about personal safety, recognizing inappropriate behavior, and knowing their legal rights. The story follows 小雨 (Xiaoyu), a young student who encounters a potentially dangerous situation and must make choices that affect the outcome.

## Current Version

**v1.4** - Latest stable version with complete graphics integration and web player

## Project Structure

```
├── 普法情景游戏_v1.4_with_tags.ink  # Current story source (Ink script)
├── story.json                        # Compiled story for web playback
├── web_player.html                   # Web-based game player
├── ink.js                            # Ink runtime (inkjs library)
├── graphics/
│   ├── background/                   # Scene backgrounds
│   │   ├── 放学路_第一幕主背景.png
│   │   ├── 堂屋.png
│   │   ├── 里屋.png
│   │   ├── 河边树林外.png
│   │   └── 派出所.png
│   └── characters/                   # Character sprites
│       ├── 小雨平静版.png
│       ├── 小雨害怕版1.png
│       ├── 小雨害怕版2.png
│       ├── 孙伯伯和蔼版.png
│       ├── 孙伯伯阴沉版.png
│       └── 女警.png
├── versions/                         # Version history
│   ├── VERSION_HISTORY.md
│   ├── v1.0/
│   ├── v1.1/
│   ├── v1.2/
│   └── v1.3/
├── archive/                          # Archived documentation
├── prd/                              # Product requirements
└── INKY OFFICIAL DOCUMENTATION/      # Ink language reference
```

## How to Play

### Option 1: Local HTTP Server (Recommended)
```bash
cd /path/to/inky
python3 -m http.server 8080
```
Then open http://localhost:8080/web_player.html in your browser.

### Option 2: Inky Editor
Open `普法情景游戏_v1.4_with_tags.ink` in [Inky](https://github.com/inkle/inky/releases) editor.

## Key Features

- **Interactive Storytelling**: Multiple branching paths based on player choices
- **Visual Novel Style**: Character sprites and background images
- **Educational Content**:
  - Legal knowledge sections (法律小课堂)
  - Safety tips (提示) displayed as expandable buttons
  - Ending reviews (结局复盘) with learning points
- **Variable System**:
  - `Risk` (hidden): Tracks danger level
  - `Harm` (visible): Tracks harm sustained
  - `Support` (visible): Tracks support received
- **Four Ending Types**:
  - A_best: Best outcome - timely help, minimal harm
  - A2_assault_reported: Assault occurred but reported
  - safe_ending_early: Escaped early
  - D_severe: Worst outcome - severe harm

## Technical Details

### Ink Tags System
The game uses custom tags for visual presentation:
- `# bg: <background_name>` - Set background image
- `# char_left: <character>` - Character on left
- `# char_center: <character>` - Character in center
- `# char_right: <character>` - Character on right
- `# mood: <mood>` - Set character mood/expression

### Compiling the Story
If you modify the .ink file, recompile using inklecate:
```bash
/Applications/Inky.app/Contents/Resources/app.asar.unpacked/main-process/ink/inkjs-compatible/inklecate_mac -o story.json 普法情景游戏_v1.4_with_tags.ink
```

### Web Player Features
- Collapsible hint boxes for educational content
- Character and background display
- Progress tracking with variable display
- Responsive design

## Documentation

| File | Description |
|------|-------------|
| `CHANGELOG.md` | Version history and changes |
| `SCENE_IMAGE_MAPPING.md` | Graphics mapping for each scene |
| `TAGS_SPECIFICATION.md` | Ink tags system documentation |
| `TESTING_GUIDE.md` | Testing instructions |
| `IMAGE_INTEGRATION_SOLUTION.md` | Technical graphics integration guide |

## Development

### Prerequisites
- [Inky Editor](https://github.com/inkle/inky/releases) for editing .ink files
- Python 3 for local server
- Modern web browser for playback

### Making Changes
1. Edit `普法情景游戏_v1.4_with_tags.ink` in Inky
2. Compile to JSON using inklecate
3. Test in browser with local server

## License

Educational use only. Content designed for child safety education in Chinese legal context.

## Credits

- Story and game design by the development team
- Built with [Ink](https://www.inklestudios.com/ink/) by Inkle Studios
- Web runtime: [inkjs](https://github.com/y-lohse/inkjs)
