# 🌸 FemFlow — Period Tracking & PCOS Management

FemFlow is a full-stack web application for **menstrual cycle tracking** and **PCOS (Polycystic Ovary Syndrome) risk management**. It combines daily health logging, statistical cycle predictions, an algorithmic PCOS risk score, and **AI-powered personalized insights** to help women understand and manage their reproductive health.

---

## Features

### 🩸 Cycle Tracking
- Log period start/end dates, flow intensity (light/medium/heavy), and notes
- Auto-calculates cycle length by comparing consecutive periods
- Auto-calculates period length from start and end dates
- View full cycle history sorted by date

### 🔮 Cycle Predictions
- Predicts next period start and end dates
- Calculates estimated ovulation date
- Determines fertile window (5 days before ovulation + 1 day after)
- Shows current cycle day and days until next period
- Uses last 6 cycles for accuracy, filters outlier data

### 📝 Daily Health Logging
- **Mood:** Great, Good, Okay, Bad, Terrible (emoji picker)
- **Symptoms:** 17 trackable symptoms including cramps, bloating, headache, acne, fatigue, mood swings, anxiety, hair loss, etc.
- **Lifestyle:** Sleep hours, water intake (glasses), exercise (minutes), stress level (1–5 slider)
- **Diet:** 9 categories — balanced, high sugar, high fat, processed food, fruits/veggies, dairy, caffeine, alcohol, skipped meal
- **Weight:** Optional daily weight tracking (kg)
- **Notes:** Free-text notes
- One log per day (upsert — updates existing entry for the same date)

### 🧬 PCOS Risk Assessment
- Algorithmic scoring system (0–100 scale)
- Three evaluation dimensions:
  - **Cycle Regularity (0–30):** Based on standard deviation of cycle lengths
  - **Symptom Frequency (0–40):** Frequency of 10 PCOS-related symptoms
  - **Lifestyle Factors (0–30):** Sleep, exercise, stress, and diet patterns
- Risk levels: Low (0–30), Moderate (31–60), High (61–100)
- Generates personalized tips based on specific weak areas
- Includes medical disclaimer — NOT a diagnostic tool

### 🤖 AI-Powered Insights (OpenRouter)
- **Motivational Lines:** Personalized, warm motivational messages with emojis, generated for each user based on their name and cycle day
- **Cycle Phase Analysis:** AI explains the current menstrual phase (follicular/ovulatory/luteal/menstrual) with body tips and wellness suggestions
- **Health Suggestions:** 4–6 prioritized, actionable health suggestions based on PCOS score, lifestyle averages, symptoms, and diet patterns
- **Daily Affirmation:** AI-generated empowering affirmation displayed on the dashboard
- Graceful fallback to hardcoded defaults if AI is unavailable

### 📊 Analytics & Charts
- **PCOS Risk Detail:** Score breakdown with visual progress bars per category
- **Cycle Length Trend:** Line chart showing how cycle lengths change over time
- **Symptom Frequency:** Horizontal bar chart of symptom occurrence (30 days)
- **Mood Distribution:** Donut/pie chart of mood breakdown (30 days)
- **Mood Trend:** Line chart mapping mood (1–5) over time
- **Sleep Trend:** Bar chart of nightly sleep hours
- **Lifestyle Averages:** Sleep, water, exercise, and stress averages (30 days)

### ⏰ Reminders
- 6 reminder types: Period, Medication, Hydration, Exercise, Sleep, Custom
- Configurable time (HH:mm) and days of the week
- Toggle reminders active/inactive
- Hourly cron job checks and triggers matching reminders
- Browser notification permission integration

### 🔐 Authentication
- Email + password registration and login
- Passwords hashed with bcrypt (10 salt rounds)
- JWT-based authentication (30-day tokens)
- Protected routes — all data endpoints require valid token
- Complete data isolation — users can only access their own data

---

## Tech Stack

| Layer      | Technology                                        |
| ---------- | ------------------------------------------------- |
| Frontend   | React 19, React Router 7, Recharts 3, Axios       |
| Bundler    | Vite 7                                            |
| Backend    | Node.js, Express.js 4                             |
| Database   | MongoDB with Mongoose 8                           |
| Auth       | JWT (jsonwebtoken) + bcryptjs                     |
| AI         | OpenRouter API via OpenAI SDK (nvidia/nemotron-3-super-120b-a12b:free) |
| Cron       | node-cron                                         |
| Icons      | React Icons (Feather Icons)                       |

---

## Project Structure

```
femflow/
├── client/                        # React frontend (Vite)
│   ├── src/
│   │   ├── App.jsx                # App shell & routes
│   │   ├── components/
│   │   │   ├── Navbar.jsx         # Navigation bar
│   │   │   └── ProtectedRoute.jsx # Auth guard
│   │   ├── context/
│   │   │   └── AuthContext.jsx    # Auth state management
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx      # Main dashboard
│   │   │   ├── Cycles.jsx         # Cycle tracking
│   │   │   ├── DailyLog.jsx       # Daily health log
│   │   │   ├── Analytics.jsx      # Charts & analytics
│   │   │   ├── Reminders.jsx      # Reminder management
│   │   │   ├── Login.jsx          # Login page
│   │   │   └── Register.jsx       # Registration page
│   │   └── services/
│   │       └── api.js             # Axios API client
│   └── package.json
│
├── server/                        # Express backend
│   ├── server.js                  # Entry point
│   ├── middleware/
│   │   └── auth.js                # JWT authentication
│   ├── models/
│   │   ├── User.js                # User model
│   │   ├── CycleLog.js            # Menstrual cycle model
│   │   ├── DailyLog.js            # Daily health log model
│   │   └── Reminder.js            # Reminder model
│   ├── routes/                    # Express route definitions
│   ├── controllers/               # Route handler logic
│   └── utils/
│       ├── geminiService.js       # OpenRouter AI integration
│       ├── cyclePrediction.js     # Cycle prediction algorithm
│       ├── pcosScoring.js         # PCOS risk scoring algorithm
│       └── reminderCron.js        # Hourly reminder checker
│
├── ARCHITECTURE.md                # Detailed architecture document
└── README.md                      # This file
```

---

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/femflow.git
cd femflow
```

### 2. Setup the Server

```bash
cd server
npm install
```

Create a `.env` file in the `server/` directory:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/femflow
JWT_SECRET=your_secure_random_secret_here
OPENROUTER_API_KEY=sk-or-v1-your-key-here
```

> **Important:** Replace `JWT_SECRET` with a strong random string in production. Get your OpenRouter API key from [openrouter.ai/keys](https://openrouter.ai/keys).

Start the server:

```bash
# Development (with auto-reload)
npm run dev

# Production
npm start
```

The API will be available at `http://localhost:5000`.

### 3. Setup the Client

```bash
cd client
npm install
```

Start the dev server:

```bash
npm run dev
```

The app will open at `http://localhost:5173`.

### 4. Build for Production

```bash
cd client
npm run build
```

The built files will be in `client/dist/`.

---

## License

This project is for educational and personal use.

---

