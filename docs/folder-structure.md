# Folder Structure

```
/conveyor-belt-nodejs-JS
│
├── lib/                      # Core simulation logic
│   ├── conveyor.js           # Conveyor belt mechanics
│   ├── workers.js            # Worker behavior system
│   ├── gpio.js               # Raspberry Pi GPIO interface
│   └── db.js                 # SQLite persistence
│
├── public/                   # Frontend assets
│   ├── index.html            # Browser interface
│   ├── styles.css            # CSS styles
│   └── script.js             # Client-side JS
│
├── test/                     # Test suites
│   ├── unit/                 # Unit tests
│   │   ├── conveyor.test.js
│   │   ├── workers.test.js
│   │   └── gpio.mock.test.js
│   │
│   └── integration/          # Integration tests
│       ├── api.test.js
│       ├── db.test.js
│       └── simulation.test.js
│
├── config/                   # Configuration files
│   ├── prometheus.yml        # Metrics scraping config
│   └── grafana-dashboard.json # Dashboard template
│
├── docs/                     # Documentation
│   ├── architecture.md
│   └── api-reference.md
│
├── index.js                  # Main application entry
├── package.json              # Dependencies & scripts
├── README.md                 # Project documentation
└── .gitignore                # Version control excludes

```

## Key Component Legend

```
  📦 lib/          - Core simulation engine
    ├─ 🏭 conveyor.js  - Belt movement logic
    ├─ 👷 workers.js   - Worker coordination
    ├─ 🔌 gpio.js      - Hardware interface
    └-- 🗃️ db.js       - Data persistence

  🌐 public/       - Browser-accessible files
    ├─ 🖼️ index.html - Web interface
    ├-- 🎨 styles.css - Visual styling
    └-- 🛠️ script.js  - Client logic

  🧪 test/         - Verification suites
    ├─ 🔬 unit/    - Isolated component tests
    └-- 🧩 integration/ - System behavior tests

  ⚙️ config/      - Operational settings
    ├-- 📊 prometheus.yml - Monitoring config
    └-- 📈 grafana-dashboard.json - Visualization

  📄 Root Files    - Project foundation
    ├-- 🚀 index.js - Application launcher
    ├-- 📝 package.json - Dependency manifest
    └-- 📖 README.md - Project guide

```