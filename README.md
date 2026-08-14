বিজ্ঞান কুইজ — Full-Stack (React + Express + MongoDB)
A production-grade quiz platform with a React frontend, Express backend, MongoDB database, and AI-powered PDF → quiz conversion.
🗂️ Monorepo Structure
quiz-fullstack/
│
├── package.json              ← root workspace (runs both with concurrently)
│
├── server/                   ← Express + MongoDB backend
│   ├── package.json
│   ├── .env.example          ← copy to .env and fill in
│   └── src/
│       ├── index.js          ← app entry, auto-seeder on first run
│       ├── config/
│       │   └── db.js         ← Mongoose connection
│       ├── models/
│       │   ├── Subject.js
│       │   ├── Level.js
│       │   ├── Question.js   ← compound index subjectId+levelId
│       │   ├── Result.js     ← denormalised for fast leaderboard
│       │   └── Settings.js   ← singleton (timerMin, quizOptions, etc.)
│       ├── middleware/
│       │   ├── auth.js       ← JWT requireAdmin guard
│       │   └── errorHandler.js
│       ├── controllers/
│       │   ├── authController.js
│       │   ├── subjectController.js
│       │   ├── levelController.js
│       │   ├── questionController.js ← CRUD + bulkCreate + export
│       │   ├── resultController.js   ← save + leaderboard + admin
│       │   ├── settingsController.js
│       │   └── pdfController.js      ← multer → pdfParser → JSON
│       ├── services/
│       │   └── pdfParser.js  ← pdf-parse + GPT-4o-mini → MCQ array
│       └── routes/
│           ├── authRoutes.js
│           ├── publicRoutes.js   ← no auth
│           └── adminRoutes.js    ← all behind requireAdmin
│
└── client/                   ← React + Vite + Tailwind + react-router v7
    ├── package.json
    ├── vite.config.js        ← /api proxy → localhost:5000
    ├── tailwind.config.js
    └── src/
        ├── main.jsx          ← RouterProvider entry
        ├── api/
        │   ├── axios.js      ← instance + JWT interceptor + 401 redirect
        │   └── index.js      ← authAPI, subjectsAPI, levelsAPI,
        │                        questionsAPI, resultsAPI, settingsAPI, pdfAPI
        ├── context/
        │   └── QuizContext.jsx  ← API-powered global state (no localStorage for data)
        ├── router/
        │   └── index.jsx     ← createBrowserRouter, loaders, guards
        ├── layouts/
        │   ├── RootLayout.jsx    ← QuizProvider + Navbar + Toast
        │   ├── PublicLayout.jsx
        │   ├── QuizFlowLayout.jsx
        │   └── AdminLayout.jsx
        ├── routes/
        │   ├── public/
        │   │   ├── HomePage.jsx
        │   │   └── LeaderboardPage.jsx   ← fetches from /api/results/leaderboard
        │   ├── quiz/
        │   │   ├── SelectSubjectPage.jsx
        │   │   ├── SelectLevelPage.jsx
        │   │   ├── JoinPage.jsx
        │   │   ├── QuizPage.jsx          ← POSTs result to /api/results
        │   │   └── ResultPage.jsx
        │   ├── admin/
        │   │   ├── AdminLoginPage.jsx    ← POST /api/auth/login → JWT
        │   │   └── AdminPage.jsx         ← 6 tabs including PDF Upload
        │   ├── NotFoundPage.jsx
        │   └── ErrorPage.jsx
        ├── components/
        │   ├── layout/   ← SciBg, SelBg, ScientistPortraits (visual only)
        │   └── shared/   ← Navbar, Toast, StepTracker
        ├── hooks/
        │   └── useTimer.js
        ├── utils/
        │   └── helpers.js
        └── styles/
            └── global.css
