# 守护少年：普法情景游戏

**Legal Education Interactive Game for Children**

An interactive fiction game built with [Ink](https://www.inklestudios.com/ink/) that teaches children about personal safety, recognizing inappropriate behavior, and knowing their legal rights. The story follows 小雨 (Xiaoyu), a young student who encounters a potentially dangerous situation and must make choices that affect the outcome.

## Current Version

**v1.4** - Complete graphics integration, web player, and AI guardian assistant

## Features

### Interactive Story
- **Branching Narrative**: Multiple paths based on player choices
- **Visual Novel Style**: Character sprites and background scenes
- **Educational Content**: Legal knowledge sections, safety tips, and ending reviews

### AI Guardian Assistant (灵灵)
- **Real-time Chat**: Ask questions about safety, law, or the story
- **Legal Knowledge RAG**: Retrieves relevant Chinese legal articles to provide accurate information
- **Context-Aware**: Understands current game state and recent story events
- **Child-Friendly**: Warm, supportive tone designed for young users

## Project Structure

```
├── web_player.html                   # Web game + AI assistant
├── story.json                        # Compiled Ink story
├── ink.js                            # Ink runtime (inkjs)
├── legal_knowledge.json              # Legal articles for AI RAG
├── 普法情景游戏_v1.4_with_tags.ink   # Ink source script
├── CHANGELOG.md                      # Version history
├── graphics/
│   ├── background/                   # Scene backgrounds
│   └── characters/                   # Character sprites
├── 知识库/                           # Source legal documents
└── archive/                          # Archived documentation
```

## How to Play

### Local Server (Required)
```bash
cd /path/to/inky
python3 -m http.server 8080
```
Open http://localhost:8080/web_player.html in your browser.

### Inky Editor (Story editing only)
Open `普法情景游戏_v1.4_with_tags.ink` in [Inky](https://github.com/inkle/inky/releases).

## Technical Details

### AI Assistant Architecture
- **Frontend**: Chat sidebar in `web_player.html`
- **API Proxy**: Cloudflare Worker (keeps API key secure)
- **Model**: Qwen/Qwen3.5-397B-A17B via SiliconFlow
- **RAG**: Keyword-based retrieval from `legal_knowledge.json`

### Ink Tags System
```ink
# bg: 放学路_第一幕主背景.png    // Set background
# char_left: 小雨平静版.png      // Left character
# char_center: 孙伯伯和蔼版.png  // Center character
# mood: tense                    // Set mood (affects UI color)
```

### Game Variables
- `silence_count`: Tracks player's silent/passive responses
- `current_act`: Current story act (1-4)
- `assault_occurred`: Boolean flag for ending determination

### Ending Types
| Ending | Description |
|--------|-------------|
| A_best | Best outcome - timely help, minimal harm |
| A2_assault_reported | Assault occurred but reported |
| safe_ending_early | Escaped early |
| D_severe | Worst outcome - severe harm |

## Development

### Prerequisites
- [Inky Editor](https://github.com/inkle/inky/releases) for .ink files
- Python 3 for local server
- Modern web browser

### Compiling Story Changes
```bash
/Applications/Inky.app/Contents/Resources/app.asar.unpacked/main-process/ink/inkjs-compatible/inklecate_mac -o story.json 普法情景游戏_v1.4_with_tags.ink
```

### Cloudflare Worker Setup
The AI assistant uses a Cloudflare Worker proxy to secure the API key:
1. Create a Worker at [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Add `SILICONFLOW_API_KEY` as a secret
3. Deploy the proxy code (see `ai_api_proxy.js` for reference)

## License

Educational use only. Content designed for child safety education in Chinese legal context.

## Credits

- Story and game design by the development team
- Built with [Ink](https://www.inklestudios.com/ink/) by Inkle Studios
- Web runtime: [inkjs](https://github.com/y-lohse/inkjs)
- AI powered by [SiliconFlow](https://siliconflow.cn)
