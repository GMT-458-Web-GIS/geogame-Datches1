# Turkish Famous GeoGame — Design Documentation (%35)

**Design Deadline:** 17 November 2024

This design document fulfills the "Design of the geo-game (%35)" requirements from the project PDF, accurately reflecting the current code implementation and behavior.

---

## 🎮 Game Overview

**FAMOUSGUESSR** is an interactive geography game where players guess the birthplaces of Turkish famous people on an interactive Turkey map. Players select difficulty levels and compete against time to earn points by correctly identifying provinces.

![Intro Screen](public/images/screenshots/intro.png)
*Main menu with difficulty selection and game information*

![Game Screen](public/images/screenshots/game.png)
*Interactive gameplay with province highlighting and real-time feedback*

![Welcome Modal](public/images/screenshots/modal.png)
*Game rules and instructions modal*

---

## 📋 Requirements & Frontend Layout

### Core Features
- **Interactive Turkey Map**: OpenLayers-based vector map with 81 provinces (GeoJSON data)
- **Difficulty Selection**: Two modes (Normal & Hard) with different time mechanics
- **Real-time Feedback**: Visual province highlighting (gray → orange options → green/red results)
- **Countdown Timer**: Dynamic time display with difficulty-based adjustments
- **Score System**: +10 points per correct answer
- **Celebrity Database**: 50+ Turkish famous people with birthplace data
- **Responsive Design**: Optimized for desktop and mobile devices

### Frontend Layout

#### Desktop Layout
```
┌─────────────────────────────────────────────────────┐
│  [🏠 Home]    [⏱️ 87s]  [🎯 0 puan]  [❓ Soru 1]   │
├─────────────────────────────────────────────────────┤
│                                                     │
│           Interactive Turkey Map                    │
│         (OpenLayers Vector Layer)                   │
│     - Gray: Unanswered provinces                    │
│     - Orange: Current question options              │
│     - Green: Correct answer (clicked)               │
│     - Red: Wrong answer (clicked)                   │
│                                                     │
├─────────────────────────────────────────────────────┤
│  ┌───────────────────────────────────────────────┐ │
│  │ [Photo] Celebrity Name                        │ │
│  │         Category (Sporcu/Oyuncu/Müzisyen)     │ │
│  │ Select the province on the map!               │ │
│  │ Options: [İzmir] [Karabük] [Mardin] [Muğla]   │ │
│  └───────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

#### Mobile Layout
```
┌──────────────────────┐
│ [🏠] [⏱️] [🎯] [❓]   │
├──────────────────────┤
│                      │
│   Turkey Map         │
│   (Touch enabled)    │
│                      │
├──────────────────────┤
│ Celebrity Info       │
│ [Options Chips]      │
└──────────────────────┘
```

---

## ❓ Required Questions & Answers

### How will the game progress?
The game features **two difficulty levels** with time-based progression:

#### **Normal Mode (90 seconds)**
- Fixed 90-second timer
- Each correct answer: **+10 points**
- Wrong answers: No time penalty
- Players answer as many questions as possible until time runs out
- Simple mechanics for casual gameplay

#### **Hard Mode (60 seconds)**
- Starts with 60 seconds
- Each correct answer: **+10 points** and **+1 second** bonus time
- Each wrong answer: **-3 seconds** time penalty
- Game ends if time reaches 0 from wrong answers
- Strategic gameplay requiring quick thinking and accuracy

### How many questions will there be?
- **Dynamic question count** based on player performance
- Questions continue until timer expires
- Typical session: **8-15 questions** depending on difficulty and speed
- No predetermined question limit
- Questions drawn randomly from celebrity database without repetition

### How many lives does a user have?
- **No life system** (0 lives)
- Game is purely **time-based**
- In Normal mode: Timer runs continuously regardless of wrong answers
- In Hard mode: Wrong answers reduce time, but no instant game over
- Focus on maximizing score within time limit

---

## 🎯 Difficulty Levels (Detailed)

| Feature | Normal Mode | Hard Mode |
|---------|------------|-----------|
| Starting Time | 90 seconds | 60 seconds |
| Correct Answer Bonus | +10 points | +10 points + 1 second |
| Wrong Answer Penalty | None | -3 seconds |
| Time Cap | Fixed 90s | Max 90s (with bonuses) |
| Strategy | Relaxed, accuracy-focused | Fast-paced, risk/reward |
| Color Feedback | Green (correct), Red (wrong) | Green (correct), Red (wrong) |

---

## 🛠️ JavaScript Libraries

### Primary Libraries
- **OpenLayers 10.7.0**: 
  - Interactive map rendering
  - Vector layer styling and hover effects
  - GeoJSON data visualization
  - Click/hover event handling
  - Province highlighting system

- **React 19.2.0**:
  - Component-based UI architecture
  - State management (useState, useEffect, useRef)
  - React Router DOM for navigation

- **Vite 7.2.0**:
  - Fast development server
  - Hot module replacement (HMR)
  - Optimized production builds

### Bonus/Advanced Features (Potential)
- **D3.js** or **Chart.js**: Post-game statistics visualization
- **Framer Motion**: Enhanced UI animations
- **Deck.gl**: Advanced 3D map visualizations (future enhancement)

---

## 🎨 Visual Design System

### Color Scheme
- **Primary**: Blue gradient (`#1e3a8a` → `#3b82f6`)
- **Brand Badge**: Red (`#ef4444`)
- **Province Colors**:
  - Default: Gray (`#9E9E9E`)
  - Options: Orange (`#FFA726`)
  - Correct: Green (`#4CAF50`)
  - Wrong: Red (`#F44336`)
  - Hover: Light gray (`#ADADAD`)

### Typography
- **Title Font**: Bold, 60px (desktop)
- **Body Font**: Regular, 18px
- **UI Elements**: Sans-serif (Arial/system fonts)

---

## 📊 Game Mechanics (Detailed Flow)

### Game Initialization
1. User selects difficulty (Normal/Hard) on intro screen
2. Welcome modal displays game rules
3. Timer starts on "Let's Start!" button click
4. First celebrity question loads

### Question Flow
```
┌─────────────────────────────────────────────┐
│ 1. Random celebrity selected                │
│    (not previously asked in session)        │
├─────────────────────────────────────────────┤
│ 2. Correct province identified              │
├─────────────────────────────────────────────┤
│ 3. Three decoy provinces generated:         │
│    - 1 far province (>400km)                │
│    - 1 medium distance (200-400km)          │
│    - 1 nearby province (<200km)             │
├─────────────────────────────────────────────┤
│ 4. Four options shuffled and displayed      │
│    as orange provinces on map               │
├─────────────────────────────────────────────┤
│ 5. Player clicks province on map            │
├─────────────────────────────────────────────┤
│ 6. Instant visual feedback:                 │
│    - Green if correct (+10 pts, +1s if Hard)│
│    - Red if wrong (-3s if Hard)             │
├─────────────────────────────────────────────┤
│ 7. Sound effect plays (true.mp3/false.mp3)  │
├─────────────────────────────────────────────┤
│ 8. Next question loads after 1s delay       │
└─────────────────────────────────────────────┘
```

### Province Highlighting System
- **Hover Effect**: All provinces show tooltip with name
- **Option Highlighting**: 4 options turn orange when question loads
- **Click Feedback**: Clicked province immediately turns green/red
- **Persistent Green**: Correct provinces remain green throughout game

---

## 📁 Project Structure

```
geogame-duolingoGIS-main/
├── public/
│   ├── images/
│   │   ├── components/       # UI element images
│   │   ├── duo/              # Duolingo-style assets
│   │   ├── players/          # Celebrity photos
│   │   └── random/           # Misc graphics
│   └── sfx/
│       ├── true.mp3          # Correct answer sound
│       └── false.mp3         # Wrong answer sound
│
├── src/
│   ├── pages/
│   │   ├── Intro.jsx         # Landing page with difficulty selection
│   │   ├── Intro.css         # Intro page styling
│   │   ├── Main.jsx          # Game logic and map rendering
│   │   └── Main.css          # Game page styling
│   │
│   ├── components/
│   │   ├── Navbar.jsx        # Top navigation bar
│   │   └── Navbar.css        # Navbar styling
│   │
│   ├── data/
│   │   ├── celebrityData.js  # 50+ celebrity database
│   │   └── turkeyProvinces.js# Province coordinates & utilities
│   │
│   ├── App.jsx               # Root component with routing
│   └── main.jsx              # React entry point
│
├── databases/
│   └── maps/
│       └── custom.geo.json   # Turkey provinces GeoJSON
│
├── package.json              # Dependencies & scripts
├── vite.config.js            # Vite configuration
└── README.md                 # This file
```

---

## 🚀 Installation & Running

### Prerequisites
- Node.js 18+ and npm

### Development Mode
```powershell
# Install dependencies
npm install

# Run development server (localhost:3000)
npm run dev
```

### Production Build
```powershell
# Build optimized production bundle
npm run build

# Preview production build
npm run preview
```

---

## 🎓 Data Structure

### Celebrity Data Format
```javascript
{
  id: 1,
  name: "Celebrity Name",
  birthProvince: "İstanbul",
  birthCity: "District",
  category: "Sporcu", // Oyuncu, Müzisyen, Sporcu
  photo: "/images/players/photo.jpg",
  difficulty: "easy", // easy, medium, hard
  coordinates: [29.0086, 41.0225]
}
```

### Province Data
- 81 Turkish provinces with coordinates
- GeoJSON polygon geometries
- Distance calculation utilities
- Province name matching with special cases

---

## 🎯 Future Enhancements (Optional)

- [ ] Leaderboard system with localStorage
- [ ] Statistics dashboard (Chart.js integration)
- [ ] Achievement badges
- [ ] Multiplayer mode
- [ ] Additional difficulty levels
- [ ] Category-specific challenges (Sports/Music/Cinema)
- [ ] 3D map visualization (Deck.gl/Cesium)

---

## 📝 Notes

- **No NYC taxi data exploration**: This is a Turkey-focused geography game
- **No multiple choice buttons**: Direct map interaction only
- **No streak system**: Pure time-based scoring
- All UI text is in English for international accessibility
- Game data updates via `celebrityData.js` without code changes

## Yerleşim (Sketch)

### Masaüstü
```mermaid
flowchart LR
  A[Navbar] --> B{{Oyun Ekranı}}
  subgraph B
    C[OpenLayers Türkiye Haritası]
    D[Soru/Hedef | Sayaç (60s) | Skor]
  end
  C -. hover: l adı tooltip .-> C
```

### Mobil
```mermaid
flowchart TB
  A[Navbar]
  B[Soru/Hedef | Sayaç | Skor]
  C[OpenLayers Harita]
  D[[Hover/Tap Tooltip]]
  A --> B --> C
  C -. tap/hover: l adı tooltip .-> D
```

## Zorunlu Sorulara Yanıtlar
- Oyun nasıl ilerleyecek?: Zaman-temelli iki mod. **Normal mod:** 90 saniye sabit süre; her tıklamadan sonra yeni hedef gelir; süre bittiğinde oyun biter. **Zor mod:** 60 saniye başlangıç; her doğru cevap +1 saniye bonus, her yanlış cevap -3 saniye ceza; süre bittiğinde oyun biter.
- Kaç soru olacak?: Süreye bağlı olarak "yetiştiği kadar". Tipik bir oturumda ~8–15 soru yanıtlanır.
- Kaç can var?: Can sistemi yok (0 can). Oyun tamamen zamana karşıdır.

## Zorluk Seviyeleri
- **Normal Mod**: 90 saniye sabit süre. Basit mekanikler, yanlış cevaplar zamanı etkilemez.
- **Zor Mod**: 60 saniye başlangıç. Her doğru cevap +1 saniye bonus, her yanlış cevap -3 saniye ceza. Daha stratejik ve hızlı düşünme gerektirir.

## JS Kütüphaneleri
- OpenLayers 10.x: Harita, vektör stilleri, hover/tıklama etkileşimleri.
- React 19.x + Vite 7.x: UI ve geliştirme ortamı.
- (Bonus, opsiyonel) D3.js veya Chart.js: Sonuç ekranında basit skor grafiği eklemek için düşünülebilir.

## Oyun Mekaniği (Detay)
- Süre: 60 saniye; geri sayım ekranda görünür.
- Hedef/Soru: Her turda bir il hedeflenir; kullanıcı haritadan doğru ili tıklar.
- Geri bildirim: Hover’da il adı; tıklamada doğru/yanlış bilgisi ve yeni hedefe geçiş.
- Skor: Doğru = +10, yanlış = +0. Süre bitiminde toplam skor gösterilir.

## Proje Yapısı (Kısa)
```
src/
  pages/
    Intro.jsx, Main.jsx   # Akış ve oyun ekranı
  components/
    Navbar.jsx            # Üst menü
  data/
    countryData.js        # ller/soru havuzu
databases/
  maps/custom.geo.json    # Türkiye illeri (GeoJSON)
public/
  images/, sfx/           # Görseller ve (varsa) sesler
```

## Kurulum ve Çalıştırma
Önkoşul: Node.js 18+

Geliştirme
```powershell
npm install
npm run dev
```

Prod derleme/önizleme
```powershell
npm run build
npm run preview
```

## Notlar
- Zorluk seviyesi, çoktan seçmeli butonlar ve can/lives yoktur; oyun yalın ve zaman-temellidir.
- Hover tooltip tüm iller için aktiftir; hedef ili bulmayı hızlandırır.
- Bonus kütüphaneler eklenirse bu dosya güncellenecektir.
