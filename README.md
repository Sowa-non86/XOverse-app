# XOverse 🎮

> A modern, real-time multiplayer Tic-Tac-Toe game built with Flutter and Node.js — featuring online rooms, a vanishing tile mechanic, cosmic UI, and live socket-based gameplay.

---

## 📱 What is XOverse?

XOverse is a mobile-first Tic-Tac-Toe game that goes beyond the classic — it features:
- **Online multiplayer** via room codes (Socket.IO)
- **Vanishing mode** for local play (tiles disappear after 5 seconds)
- A cinematic, space-themed UI with animated star fields, neon glow effects, and smooth transitions

Built with Flutter (Dart) on the frontend and Node.js + Socket.IO on the backend.

---

## 🚀 Features

### 🌐 Online Multiplayer (Play Online with Friends)
- One player **creates a room** with a name → gets a 6-digit room code
- Second player **joins** using that code
- Real-time gameplay via WebSocket (Socket.IO)
- Animated pre-game sequence: opponent joined → role reveal → FIGHT!
- **Rematch system** — both players can request/accept/decline a rematch
- Scores persist across rematches within a session
- Opponent disconnect is handled gracefully

### 🖥️ Local Multiplayer — Vanishing Mode
- Two players on the same device
- Every tile you place **disappears after 5 seconds** — forces strategic thinking
- Win/loss tracked across sessions (X wins vs O wins)
- Confetti explosion on win

### 🌍 Play Worldwide *(coming soon)*
- Route placeholder for a global matchmaking mode

### 👤 Profile Screen
- Displays local and online win/loss stats
- Overall win rate calculation
- Cosmic Warrior badge system *(expandable)*

### 🔐 Auth Flow
- Email/password login and signup UI (Firebase-ready, logic stubbed)
- Google and Instagram OAuth buttons (logic stubbed)
- Animated splash screen with pixel dust effect

---

## 🗂️ Project Structure

```
lib/
├── config/
│   ├── routes.dart          # Named route map
│   └── theme.dart           # App-wide dark theme (Material 3)
│
├── screens/
│   ├── splash_screen.dart
│   ├── auth/
│   │   ├── login_screen.dart
│   │   ├── signup_screen.dart
│   │   └── profile_screen.dart
│   ├── home_screen.dart
│   ├── game/
│   │   └── local_game_screen.dart
│   └── online/
│       ├── create_n_join_room_screen.dart
│       └── online_game_screen.dart
│
├── widgets/
│   ├── game_board.dart         # Local game board with vanishing logic
│   ├── online_game_board.dart  # Online game board with socket events
│   ├── game_tile.dart          # Animated X/O tile widget
│   └── custom_button.dart      # Reusable button component
│
└── main.dart

server/
└── server.js                   # Node.js + Socket.IO backend
```

---

## 🎨 Design System

| Token | Value | Usage |
|---|---|---|
| X Color | `#00E5FF` | Cyan / Player X |
| O Color | `#FF2C93` | Pink / Player O |
| Background Dark | `#121421` | Deep space base |
| Background Mid | `#1E1E2C` | Nebula dark |
| Background Light | `#2A2D3E` | Card surfaces |
| Text | `#E0E0E0` | Star light white |

Font: **Poppins** — all weights

---

## 🧠 Game Logic

### Vanishing Mode (Local)
- On each move, a `Timer` is set for **5 seconds**
- When the timer fires, that tile is cleared (`board[index] = ''`)
- If a win is detected before the timer fires, all timers are cancelled
- This creates a dynamic board where older moves disappear — you must think ahead

### Win Detection
Standard 8-line check (3 rows + 3 cols + 2 diagonals). Called after every move on both local and online boards.

### Online Turn System
- Host is always **X**, goes first
- Joiner is always **O**
- `isMyTurn` flag flips on every confirmed move
- Moves are emitted via `play-move` → server relays as `opponent-move` to the other client only (not echoed back)

---

## 🔌 Socket Events

| Event | Direction | Description |
|---|---|---|
| `create-room` | Client → Server | Host creates a room with code + name |
| `join-room` | Client → Server | Joiner connects using room code |
| `both-joined` | Server → Both | Triggers the pre-game animation sequence |
| `play-move` | Client → Server | Sends move index + symbol |
| `opponent-move` | Server → Opponent | Relays move to the other player only |
| `game-over` | Client ↔ Server | Winner emits; server updates score + relays to loser |
| `request-rematch` | Client → Server | One player requests rematch |
| `rematch-requested` | Server → Opponent | Notifies opponent of request |
| `rematch-accepted` | Server → Both | Both agreed — reset game |
| `decline-rematch` | Client → Server | One player declines |
| `rematch-declined` | Server → Opponent | Opponent is notified, game ends |
| `opponent-left` | Server → Remaining | Triggered on socket disconnect |

---

## 🖥️ Backend

Built with **Node.js**, **Express**, and **Socket.IO**.

- Rooms stored in a `Map` — in-memory, no database
- Scores (`xWins`, `oWins`) tracked per room across rematches
- Deployed on **Render** — URL configured in `online_game_screen.dart`

### Run locally:
```bash
cd server
npm install
node server.js
# Runs on port 3000
```

Health check: `GET /health` returns active room count + timestamp.

---

## 📦 Flutter Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  socket_io_client: # Real-time communication
  confetti:         # Win celebration effect
```

*(Full pubspec.yaml not included — add these packages)*

---

## ⚙️ Setup & Run

### Flutter App
```bash
flutter pub get
flutter run
```

Change the socket URL in `online_game_screen.dart`:
```dart
final String socketUrl = "https://your-server-url.com";
```

### Backend
```bash
npm install
node server.js
```

---

## 🔧 What's Stubbed / TODO

- [ ] Firebase Auth integration (email/password, Google, Instagram)
- [ ] Persistent stats (local storage or Firestore)
- [ ] Global matchmaking ("Play Worldwide" route)
- [ ] Profile avatar upload
- [ ] Push notifications for rematch requests

---

## 👨‍💻 Author

**Made by Swanand** — `@snedzamusic`

---

## 📄 License

Private repository. All rights reserved.
