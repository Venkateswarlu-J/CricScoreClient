# LocalScore – Live Cricket Scoring for Local Matches

A professional, mobile-first web application for scoring local cricket matches and sharing live scoreboards. Built with React + Vite, Node.js + Express, MongoDB, and Socket.IO for real-time updates.

## 🏏 Features

### Core Scoring
- **Ball-by-ball delivery recording** with full cricket rules support
- **Accurate scoring engine** handling runs, boundaries, wickets, extras, and dismissals
- **Real-time live updates** via Socket.IO for spectators
- **Undo support** to reverse incorrect deliveries and restore match state
- **Duplicate prevention** with idempotency keys to protect against double-tap errors

### Match Management
- **Standalone and tournament matches** with flexible team setup
- **Configurable overs** (5, 6, 8, 10, 15, 20, or custom)
- **Optional player rosters** — quick setup without mandatory player accounts
- **Toss and innings management** with automatic transition handling
- **Match completion** with final result and scorecard archival

### Viewing & Sharing
- **Public read-only scoreboard** viewable via shareable link (no login required)
- **Responsive design** optimized for mobile, tablet, and desktop
- **Reconnection handling** with automatic state resync
- **Live status indicators** (LIVE, UPCOMING, INNINGS BREAK, COMPLETED)

### Tournament Support
- **Multi-match tournaments** (tri-series, small leagues)
- **Configurable points system** (win/loss/tie/no-result values)
- **Automatic points table** calculation
- **Match fixtures** with live status tracking

## 🛠 Tech Stack

**Frontend:**
- React 18 + Vite (fast dev server & optimized builds)
- Tailwind CSS v3 (utility-first styling)
- React Router (page navigation)
- Socket.IO client (real-time updates)
- Axios (HTTP requests)
- Lucide React (icons)
- React Hot Toast (notifications)

**Backend:**
- Node.js + Express (REST API & WebSocket server)
- MongoDB Atlas (cloud database)
- Socket.IO (real-time delivery broadcasts)
- Mongoose (schema validation & ODM)
- SHA-256 token hashing (scorer authorization)
- Express Rate Limiter (API protection)
- Helmet (security headers)

**Testing:**
- Jest (unit tests for scoring engine)
- 24 passing tests covering all cricket rules

## 📁 Project Structure

```
LocalScore/
├── client/                      # React frontend
│   ├── src/
│   │   ├── pages/               # 9 page components
│   │   │   ├── HomePage.jsx
│   │   │   ├── CreateMatchPage.jsx
│   │   │   ├── MatchSetupPage.jsx
│   │   │   ├── ScorerPage.jsx (main scoring interface)
│   │   │   ├── LiveScorePage.jsx (public)
│   │   │   ├── MatchSummaryPage.jsx
│   │   │   ├── CreateTournamentPage.jsx
│   │   │   ├── TournamentPage.jsx
│   │   │   └── NotFoundPage.jsx
│   │   ├── components/
│   │   │   ├── layout/          # Header, Layout wrapper
│   │   │   ├── common/          # StatusBadge, Spinners, Modals
│   │   │   ├── match/           # MatchCard, ScoreHeader, ScorecardTable
│   │   │   └── scorer/          # RunButtons, ExtrasPanel, WicketFlow
│   │   ├── hooks/
│   │   │   ├── useSocket.js     # Socket.IO connection
│   │   │   └── useMatch.js      # Match data + real-time sync
│   │   ├── api/
│   │   │   ├── index.js         # Axios config with auth interceptor
│   │   │   ├── matchApi.js      # Match endpoints
│   │   │   └── tournamentApi.js # Tournament endpoints
│   │   ├── context/
│   │   │   └── SocketContext.jsx # Socket.IO provider
│   │   ├── App.jsx              # Routes
│   │   ├── main.jsx             # React root
│   │   └── index.css            # Tailwind + global styles
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── package.json
│   └── dist/                    # Production build output
│
├── server/                      # Node.js backend
│   ├── src/
│   │   ├── index.js             # Express + Socket.IO setup
│   │   ├── config/
│   │   │   ├── db.js            # MongoDB connection
│   │   │   └── env.js           # Environment loader
│   │   ├── models/
│   │   │   ├── Team.js
│   │   │   ├── Player.js
│   │   │   ├── Tournament.js
│   │   │   ├── Match.js         # Full match with nested innings
│   │   │   └── DeliveryEvent.js # Ball-by-ball events
│   │   ├── services/
│   │   │   ├── scoringEngine.js (★ CORE LOGIC)
│   │   │   ├── matchService.js
│   │   │   └── tournamentService.js
│   │   ├── routes/
│   │   │   ├── matchRoutes.js   # 13 match endpoints
│   │   │   └── tournamentRoutes.js
│   │   ├── middleware/
│   │   │   ├── auth.js          # Scorer token validation
│   │   │   ├── errorHandler.js
│   │   │   └── rateLimiter.js
│   │   ├── socket/
│   │   │   └── matchSocket.js   # Room management & broadcasts
│   │   ├── utils/
│   │   │   ├── overs.js         # Cricket notation (8.2 format)
│   │   │   ├── generateToken.js
│   │   │   └── idempotency.js
│   │   └── __tests__/
│   │       └── scoringEngine.test.js (24 passing tests)
│   ├── .env.example
│   ├── package.json
│   └── node_modules/

└── README.md (this file)
```

## 🚀 Getting Started

### Prerequisites
- Node.js 18+ and npm
- MongoDB Atlas account (free tier works)
- Git

### Installation

1. **Clone or navigate to the project:**
   ```bash
   cd D:/204/L24CS204/Projects/ClaudeWorks/LocalScore
   ```

2. **Backend setup:**
   ```bash
   cd server
   npm install
   
   # Create .env from .env.example
   cp .env.example .env
   ```
   
   **Edit `.env` with your values:**
   ```env
   PORT=5000
   MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@cluster.mongodb.net/localscore
   CORS_ORIGIN=http://localhost:5173
   SCORER_TOKEN_SECRET=your-secret-key-here
   NODE_ENV=development
   ```
   
   Replace:
   - `USERNAME:PASSWORD` with your MongoDB Atlas credentials
   - `cluster` with your actual cluster name
   - `your-secret-key-here` with any random string (e.g., `$(openssl rand -hex 32)`)

3. **Frontend setup:**
   ```bash
   cd ../client
   npm install
   ```

### Running Locally

**Terminal 1 — Start the backend:**
```bash
cd server
npm start
# Server runs on http://localhost:5000
```

**Terminal 2 — Start the frontend dev server:**
```bash
cd client
npm run dev
# Frontend runs on http://localhost:5173
```

Open http://localhost:5173 in your browser.

### Running Tests

**Backend tests (scoring engine):**
```bash
cd server
npm test
```

Expected output: **24 tests passing** ✓

### Production Build

**Frontend:**
```bash
cd client
npm run build
# Output: dist/
```

**Backend** is production-ready as-is (no build step required).

## 🎮 Usage

### Creating a Match

1. Click **"Create Match"** on the home page
2. Select **Standalone** or attach to a **Tournament**
3. Enter **Team A** and **Team B** names
4. Choose **Overs** (use preset buttons or enter custom value)
5. Optionally add **venue** and **date**
6. Click **Create**
7. You'll receive:
   - **Public URL** (share with spectators) — read-only
   - **Scorer Link** (keep private) — allows you to score

### Scoring a Match

1. Open the **Scorer Link** (includes authentication token in URL)
2. Confirm **toss details**, opening batters, and bowler
3. Click **"Start Match"**
4. Use the **scoring console**:
   - **Run buttons** (0–6): tap to record runs
   - **Extras**: tap Wide/No Ball/Bye/Leg Bye for additional options
   - **Wicket**: opens modal to select dismissal type and new batter
   - **Undo**: reverses the last delivery
   - **Bowler/End Over**: available when 6 legal balls are bowled
5. **Automatic transitions**:
   - End of over: strike swaps, prompted for new bowler
   - All out or overs complete: innings ends
   - Innings 2 target reached: match ends
6. After match: view final scorecard

### Viewing Live Score

1. Open the **public match URL** (no login needed)
2. See **real-time updates** as the scorer records deliveries
3. View:
   - Current batting team score, wickets, overs
   - Current batters and bowler stats
   - Recent balls strip (color-coded)
   - Full scorecard tabs
   - Target/RRR for chase innings
4. **Auto-reconnect** if connection drops

### Creating a Tournament

1. Click **"Create Tournament"** (from home page)
2. Enter tournament name and teams
3. Set **points configuration** (defaults: W=2, L=0, T=1, NR=1)
4. Create matches linked to the tournament
5. View **fixtures**, **results**, and **automatic points table**

## 📊 Cricket Scoring Rules Implemented

### Deliveries
- ✓ Legal deliveries (dot balls, 1–6 runs)
- ✓ Boundaries (4s and 6s)
- ✓ Wides (+1 to bowling team extras, ball not legal, no strike rotation)
- ✓ No-balls (+1 to bowling team extras, ball not legal, batter runs count)
- ✓ Byes (legal delivery, runs to extras, no batter runs)
- ✓ Leg-byes (legal delivery, runs to extras, no batter runs)

### Wickets & Dismissals
- ✓ Bowled, Caught, Run Out, Stumped, LBW
- ✓ Hit Wicket, Caught & Bowled, Retired
- ✓ Wicket restrictions: only run-out on no-ball or wide
- ✓ Automatic new batter prompt after dismissal

### Strike & Overs
- ✓ Strike rotation on odd runs off bat
- ✓ Strike rotation on odd bye/leg-bye runs
- ✓ Strike swap at end of over (6 legal balls)
- ✓ Cricket notation overs (e.g., "8.2" = 8 overs, 2 legal balls)

### Innings & Match
- ✓ Innings completion: all out (10 wickets) or overs finished
- ✓ Chase completion: target reached in innings 2
- ✓ Run rate and required run rate calculations
- ✓ Match result determination

### Data Integrity
- ✓ Undo: reverses last delivery and rebuilds match state from delivery history
- ✓ Duplicate prevention: idempotency keys protect against double-tap errors
- ✓ Server validation: all mutations checked server-side before persisting
- ✓ Atomic updates: delivery and state change together, or not at all

## 🔐 Security

- **Scorer Authorization**: High-entropy tokens (32-byte random) hashed with SHA-256
  - Never exposed in API responses, only in initial match creation
  - Sent via `x-scorer-token` header on authenticated requests
- **Public Read-Only**: Viewer URLs cannot perform scoring operations
- **Rate Limiting**:
  - Mutations: 30 requests/minute
  - Reads: 100 requests/minute
- **CORS**: Restricted to configured frontend origin
- **Helmet**: Security headers (CSP, X-Frame-Options, etc.)
- **No Secrets in Logs**: Tokens and credentials never logged
- **Input Validation**: All payloads validated on server before processing

## 📱 Responsive Design

- **Mobile-first**: Primary design target for scorers standing beside the ground
- **Touch targets**: All buttons ≥ 44×44 CSS pixels
- **Scorer console**: Vertical stack on narrow widths, side-by-side on larger screens
- **Large typography**: Score text scales from mobile to desktop
- **Dark theme**: High contrast, readable in daylight

## 🎨 Design System

- **Background**: `#0f172a` (dark navy)
- **Cards**: `#1e293b` (slate-800)
- **Accent**: `#14b8a6` (teal-500)
- **Text**: `#f8fafc` (slate-50 on dark)
- **Secondary**: `#94a3b8` (slate-400)
- **Live badge**: Pulsing red with dot
- **Button corners**: 6–8px border-radius
- **Spacing**: 4px scale (p-2 = 8px, p-4 = 16px, etc.)

## 📡 Real-Time Updates (Socket.IO)

- Clients subscribe to match room: `match:{publicId}`
- **After delivery recorded**, server broadcasts:
  ```json
  {
    "type": "scoreUpdate",
    "match": { /* full authoritative match state */ }
  }
  ```
- **Reconnection**: client auto-joins room and fetches latest state
- **Public viewers**: read-only subscription, cannot emit scoring events

## 🧪 Testing

### Backend Tests
```bash
cd server
npm test
```

**Test coverage:**
- Dot balls, 1–6 runs, boundaries
- Wides, no-balls, byes, leg-byes
- Wickets (all dismissal types)
- Strike rotation (odd/even runs, end of over)
- Over notation and calculations
- Innings completion (all out, overs finished)
- Chase completion and match result
- Undo and state rebuild
- Duplicate prevention with idempotency keys
- Run rate and required run rate

### Manual Testing Checklist

- [ ] Create standalone match → see public URL and scorer token
- [ ] Access scorer link → complete toss → start match
- [ ] Record deliveries: dots, runs, wides, no-balls, extras
- [ ] Dismiss a batter → select dismissal type and new batter
- [ ] End of over → automatic strike swap, prompt for new bowler
- [ ] Complete innings → transition to innings 2 (or end if 1st)
- [ ] Finish match → view final scorecard
- [ ] Open public URL → see live score update in real-time
- [ ] Disconnect and reconnect public viewer → score auto-syncs
- [ ] Undo last delivery → state correctly restored
- [ ] Mobile device → scorer controls fit thumb reach, responsive layout
- [ ] Create tournament → add matches, view points table

## 📚 API Endpoints

### Matches

| Method | Endpoint | Purpose | Auth |
|--------|----------|---------|------|
| POST | `/api/matches` | Create match | None |
| GET | `/api/matches` | List matches | None |
| GET | `/api/matches/:matchId` | Get public match | None |
| POST | `/api/matches/:matchId/setup` | Set toss & lineup | Token |
| POST | `/api/matches/:matchId/start` | Start match | Token |
| POST | `/api/matches/:matchId/deliveries` | Record delivery | Token |
| POST | `/api/matches/:matchId/undo` | Undo last delivery | Token |
| POST | `/api/matches/:matchId/select-batter` | Select new batter | Token |
| POST | `/api/matches/:matchId/change-bowler` | Change bowler | Token |
| POST | `/api/matches/:matchId/start-innings` | Start innings 2 | Token |
| POST | `/api/matches/:matchId/complete` | Complete match | Token |
| GET | `/api/matches/:matchId/scorecard` | Full scorecard | None |

### Tournaments

| Method | Endpoint | Purpose | Auth |
|--------|----------|---------|------|
| POST | `/api/tournaments` | Create tournament | None |
| GET | `/api/tournaments` | List tournaments | None |
| GET | `/api/tournaments/:id` | Tournament + standings | None |

## 🐛 Troubleshooting

### Frontend won't connect to backend
- Check backend is running on `http://localhost:5000`
- Verify `CORS_ORIGIN` in server `.env` matches frontend URL
- Check browser console for CORS errors

### Database connection fails
- Verify MongoDB URI in `.env` is correct
- Check MongoDB Atlas IP whitelist includes your machine
- Ensure network connectivity to MongoDB Atlas

### Tests fail
- Run `npm test` from `server/` directory
- Ensure Jest is installed: `npm install`
- Check Node version (18+ required)

### Deliveries not updating in real-time
- Check Socket.IO connection in browser DevTools (Network → WS)
- Verify backend is broadcasting to match room
- Try refreshing the public viewer page

## 🚀 Deployment

### Frontend (Vercel)

1. Push code to GitHub repository
2. Connect repo to Vercel
3. Set environment variables:
   ```
   VITE_API_URL=https://your-backend.com
   ```
4. Deploy (`npm run build` runs automatically)

### Backend (Railway, Render, or Heroku)

1. Create account and new project
2. Connect GitHub repository
3. Set environment variables in dashboard:
   ```
   PORT=5000
   MONGODB_URI=mongodb+srv://...
   CORS_ORIGIN=https://your-frontend.vercel.app
   SCORER_TOKEN_SECRET=...
   NODE_ENV=production
   ```
4. Deploy (detects Node.js automatically)

**Important**: Backend must support **long-lived WebSocket connections** for Socket.IO. Railway, Render, and cloud.railway.app all support this.

## 📄 Environment Variables

### Server (`.env`)

```env
# Port the backend runs on
PORT=5000

# MongoDB Atlas connection string
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/localscore

# Frontend URL for CORS
CORS_ORIGIN=http://localhost:5173

# Secret for token hashing (use any random string)
SCORER_TOKEN_SECRET=your-random-secret-key

# Environment mode
NODE_ENV=development
```

### Frontend

Frontend doesn't need a `.env` — it uses relative API URLs in development (via Vite proxy) and absolute URLs in production.

## 📝 License

This project is for local cricket scoring and recreational use.

## 🤝 Contributing

Feel free to extend the project with:
- Player profiles and career stats
- Advanced analytics and graphs
- SMS/email match notifications
- Multi-language support
- Native mobile apps
- DLS calculations for rain-affected matches

---

**Built with ❤️ for local cricket communities.**

## 📜 Credits & Copyright

**Built by:** JV (Javvaji Venkateswarlu)  
**Copyright © 2026 JV**

All rights reserved. LocalScore is developed and maintained by JV for cricket match scoring and community engagement.

For questions or issues, refer to the code comments and test cases for guidance on how the scoring engine works.
