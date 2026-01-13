# LoL Session Tracker v3.2.3

Track your League of Legends ranked sessions with real-time LP tracking, Spotify integration, and professional overlay for streaming.

## 🎉 What's New in v3.2.3

### 🎵 Spotify Integration
- **Real-time song display** in overlay
- **Scrolling text animation** (20s smooth scroll)
- **Toggle on/off** from main interface
- **Cross-platform detection** (Windows/macOS/Linux)
- **Low resource usage** (<0.1% CPU, <5MB RAM)
- **Auto-updates** every 3 seconds

### 🎨 Enhanced Overlay Design
- **Spotify widget** positioned below rank icon
- **Optimized font sizes** for better readability
- **Clean layout** with no overlapping elements
- **Professional appearance** ready for streaming

### 🔧 Technical Improvements
- **Simplified LP calculation** for Season 2025 (no promotions system)
- **Improved overlay performance** 
- **Better error handling** for Spotify detection
- **Persistent Spotify state** (remembers on/off preference)

---

## ✨ Features

### 📊 Session Tracking
- **Real-time LP tracking** with automatic detection
- **Win/Loss counter** for current session
- **Net LP calculation** (accurate for Season 2025)
- **Session winrate** percentage
- **Total winrate** with W-L record

### 🎮 Multi-Account Support
- **MULTI mode** - Track multiple accounts in rotation
- **Auto-switching** every 30 seconds
- **Visual transitions** between accounts
- **Independent session stats** per account
- **Easy account management** (add/remove/activate)

### 🔥 TryHard Mode
- **Red theme** for intense sessions
- **Visual indicator** across all interfaces
- **Toggle on/off** anytime
- **Persistent state** between sessions

### 🎵 Spotify Integration (NEW!)
- **Current track display** in overlay
- **Artist - Song format** 
- **Smooth scrolling animation**
- **Auto-detection** of playback
- **Works with**: Spotify Desktop (Windows/macOS/Linux)

### 📺 Streaming Ready
- **Electron overlay** - Draggable, transparent window
- **OBS overlay** - Browser source compatible
- **Auto-hide** when League is closed
- **Click-through** overlay
- **Professional design** with rank icons

---

## 🚀 Installation

### Prerequisites
- **Node.js** 18+ 
- **Riot Games API Key** ([Get it here](https://developer.riotgames.com/))
- **League of Legends** installed
- **Spotify Desktop** (optional, for Spotify feature)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/lol-session-tracker.git
   cd lol-session-tracker
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure API Key**
   - Open `config.json`
   - Add your Riot API key:
   ```json
   {
     "riotApiKey": "RGAPI-YOUR-KEY-HERE"
   }
   ```

4. **Start the application**
   ```bash
   npm start
   ```

---

## 🎮 Usage

### Adding Your First Account

1. **Click "Add Account"** in the main interface
2. **Enter summoner name** (e.g., `Faker`)
3. **Select region** (e.g., `KR`, `NA1`, `EUW1`)
4. **Click Add** - The account will be verified and added

### Starting a Session

1. **Select an account** from the list
2. **Click "Set Active"** to start tracking
3. **Play ranked games** - LP is tracked automatically
4. **View stats** in real-time on the overlay

### MULTI Mode (Track Multiple Accounts)

1. **Add multiple accounts** to your list
2. **Click "MULTI MODE"** button
3. **Overlay rotates** between accounts every 30s
4. **Each account** maintains independent session stats

### Spotify Integration

1. **Open Spotify Desktop** and play a song
2. **Click "🎵 Mostrar Spotify"** in tracker
3. **Current song appears** in overlay below rank icon
4. **Auto-updates** when you change songs

---

## 🎥 OBS Studio Setup

### Adding the Overlay to OBS

1. **Open OBS Studio**
2. **Add Browser Source**:
   - Sources → Add → Browser
   - Name: "LoL Session Tracker"

3. **Configure Browser Source**:
   ```
   ☑ Local file
   URL: C:\path\to\lol-session-tracker\obs\overlay.html
   
   Width: 340
   Height: 150
   
   ☑ Shutdown source when not visible
   ```

4. **Start the tracker app** (generates `data.json` automatically)
5. **Overlay updates** every 2 seconds

### Positioning Tips
- **Top-left corner**: Classic placement
- **Bottom-left**: Doesn't block facecam
- **Over gameplay**: Use transparency (Ctrl+T in OBS)

---

## 📊 How LP Tracking Works

### Season 2025 (Current)
Riot Games **removed promotions** between divisions:
- **Old**: D3 95LP + Win → D2 75LP (reset to 75)
- **New**: D3 95LP + Win (+30LP) → D2 20LP (adds LP directly)

### LP Calculation Formula

**Division Promotion** (D3 → D2):
```javascript
lpDiff = (100 - lpBefore) + lpAfter
// Example: D3 90LP → D2 20LP = +30 LP
```

**Division Demotion** (D2 → D3):
```javascript
lpDiff = -(lpBefore + (100 - lpAfter))
// Example: D2 5LP → D3 85LP = -20 LP
```

**Same Division**:
```javascript
lpDiff = lpAfter - lpBefore
// Example: D2 50LP → D2 75LP = +25 LP
```

---

## 🎵 Spotify Integration Details

### Supported Platforms
- ✅ **Windows** - PowerShell window title detection
- ✅ **macOS** - AppleScript integration
- ✅ **Linux** - MPRIS/dbus detection

### How It Works
1. **Detection Module** (`src/spotifyDetector.js`)
   - Polls every 3 seconds
   - Reads current track from Spotify
   - Formats as "Artist - Song Title"

2. **Backend Integration** (`main.js`)
   - Stores current track in memory
   - Sends updates to overlay via IPC
   - Includes track in `data.json` for OBS

3. **Frontend Display** (`overlay.html`)
   - Shows track below rank icon
   - Scrolling text animation (20s cycle)
   - Auto-hides when no track playing

### Troubleshooting Spotify

**Song not detected:**
- Ensure Spotify Desktop is open (not web player)
- Check that a song is actively playing
- Wait 3 seconds for next detection cycle

**No text visible:**
- Toggle Spotify off and on again
- Check that Spotify window has a title (not paused)
- Restart the tracker app

---

## 🏗️ Architecture

### File Structure
```
lol-session-tracker/
├── src/
│   ├── riotApi.js              # Riot API integration
│   ├── sessionManager.js       # Session state management
│   ├── accountManager.js       # Account CRUD operations
│   ├── multiModeManager.js     # MULTI mode logic
│   ├── multiSessionManager.js  # Per-account session tracking
│   └── spotifyDetector.js      # Spotify integration (NEW)
├── app/
│   ├── index.html              # Main interface
│   ├── overlay.html            # Electron overlay
│   └── renderer.js             # Frontend logic
├── obs/
│   └── overlay.html            # OBS overlay (reads data.json)
├── locales/
│   ├── es.json                 # Spanish translations
│   └── en.json                 # English translations
├── main.js                     # Electron main process
├── preload.js                  # IPC bridge
└── config.json                 # Configuration
```

---

## 🌐 Supported Regions

- **NA1** - North America
- **EUW1** - Europe West
- **EUN1** - Europe Nordic & East
- **KR** - Korea
- **BR1** - Brazil
- **LA1** - Latin America North
- **LA2** - Latin America South
- **OC1** - Oceania
- **RU** - Russia
- **TR1** - Turkey
- **JP1** - Japan
- **PH2** - Philippines
- **SG2** - Singapore
- **TH2** - Thailand
- **TW2** - Taiwan
- **VN2** - Vietnam

---

## 📝 Changelog

### v3.2.3 (Current)
- ✅ Spotify integration with scrolling text
- ✅ LP calculation fix for Season 2025
- ✅ Improved overlay font sizes
- ✅ Cross-platform Spotify detection
- ✅ OBS overlay with Spotify support

### v3.1.2
- MULTI mode with per-account sessions
- Visual transitions between accounts
- Session independence in MULTI mode

### v3.0.0
- TryHard mode theme
- Improved overlay design
- Better error handling

---

## 📜 License

MIT License - feel free to use this project for personal or commercial purposes.

---

## 🙏 Credits

- **Riot Games** - For the League of Legends API
- **Electron** - Cross-platform desktop framework
- **Spotify** - Music integration
- **Community** - For feedback and suggestions

---

## 🎮 Enjoy Your Ranked Climb!

Made with ❤️ for the League of Legends community

Track your progress, improve your gameplay, and dominate the Rift! 🏆
