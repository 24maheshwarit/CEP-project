# SOS Analytics Dashboard

A **frontend-only** React dashboard for tracking student performance across 3 years (2023–2025) using `tea_score` data from School of Scholars (SOS), Maharashtra.

---

## ✨ Features

| Category | Features |
|---|---|
| **KPIs** | Avg score per year, highest scorer, most improved/declined |
| **Charts** | Line chart (student progression), Bar chart (grade × year), Year comparison, Score distribution, 2023 vs 2025 scatter |
| **Student Analysis** | Top 10 improvers, declining students, trend badges |
| **Students Table** | Sortable, paginated (20/page), expandable rows, full score details |
| **Filters** | Search by name/ID, filter by year/grade/school |
| **Schools** | Per-school averages, city breakdown, performance comparison |
| **Export** | Download filtered students to CSV |
| **UI** | Dark/light mode toggle, responsive layout, sidebar navigation |

---

## 🗂 Project Structure

```
sos-dashboard/
├── public/
│   ├── favicon.svg
│   └── data/
│       ├── 2023.csv          ← SOS 2023 results (auto-loaded)
│       ├── 2024.csv          ← SOS 2024 results (auto-loaded)
│       └── 2025.csv          ← SOS 2025 results (auto-loaded)
├── src/
│   ├── main.jsx              ← React entry point
│   ├── App.jsx               ← Root component + page routing
│   ├── index.css             ← Tailwind + global styles
│   ├── hooks/
│   │   └── useData.js        ← Fetches, parses, and processes CSVs
│   ├── utils/
│   │   ├── dataProcessor.js  ← Merges datasets, computes KPIs
│   │   └── exportUtils.js    ← CSV download helper
│   └── components/
│       ├── Sidebar.jsx
│       ├── KPICards.jsx
│       ├── ProgressionChart.jsx
│       ├── BarCharts.jsx
│       ├── TopStudents.jsx
│       ├── StudentTable.jsx
│       ├── Filters.jsx
│       ├── SchoolsPage.jsx
│       ├── TrendsPage.jsx
│       ├── LoadingScreen.jsx
│       └── FileUpload.jsx
├── index.html
├── package.json
├── vite.config.js
├── tailwind.config.js
└── postcss.config.js
```

---

## 🚀 Quick Start

### Prerequisites
- **Node.js** 18+ (check: `node -v`)
- **npm** 9+ (check: `npm -v`)

### Step-by-step

```bash
# 1. Navigate into the project
cd sos-dashboard

# 2. Install dependencies  (~30 seconds)
npm install

# 3. Start development server
npm run dev
```

4. Open **http://localhost:5173** in your browser.

The dashboard auto-loads data from `public/data/2023.csv`, `2024.csv`, and `2025.csv`.

### Build for Production

```bash
npm run build        # creates dist/ folder
npm run preview      # serve the production build locally
```

---

## 📊 CSV Format

All 3 CSV files must contain these columns:

| Column | Type | Description |
|---|---|---|
| `usr_username` | string | **Unique student ID** (used to link across years) |
| `Name` | string | Student full name |
| `Standard` | number | Grade (3–12) |
| `tea_score` | number | TEA performance score |
| `sch_name` | string | School name |
| `sch_city` | string | School city |
| `Division` | string | Class division (A, B, etc.) |
| `ale_name` | string | Achievement category |
| `tea_rank` | number | Overall rank |
| `tea_rank_school` | number | Rank within school |
| `usr_gender` | string | M / F |
| `Age_Group` | string | Age bracket |

### Sample rows:
```csv
usr_username,Name,Standard,tea_score,sch_name,sch_city,Division,ale_name,tea_rank,tea_rank_school,usr_gender,Age_Group
jdoe0001,Jane Doe,5,78,School of Scholars Nagpur,Nagpur,A,First in School,12,1,F,10-12
asmith002,Aryan Smith,6,55,School of Scholars Amravati,Amravati,B,Participant,245,18,M,11-13
```

---

## 🔄 Using Your Own Files (Upload Mode)

If the CSVs are not found in `public/data/`, the dashboard switches to **Upload Mode** automatically. Simply:

1. Export your Excel files to CSV format
2. Drag-and-drop each year's CSV into the upload slots
3. Click "Load Dashboard"

---

## 🧠 Data Processing Logic

```
2023.csv ──┐
2024.csv ──┼──► processAllData() ──► unified students[] ──► all charts & tables
2025.csv ──┘
```

**Merging**: Students are matched across years by `usr_username`.

**Grade Progression**: The `Standard` column in each year's data tracks actual grade (e.g., 5 in 2023, 6 in 2024, 7 in 2025).

**Improvement Score**: `latest_year_score − earliest_year_score`

**Trend** classification:
- `▲ Up` — improved by more than 2 points
- `▼ Down` — declined by more than 2 points  
- `→ Flat` — change within ±2 points
- `• New` — only present in 1 year

---

## 🛠 Tech Stack

| Library | Version | Purpose |
|---|---|---|
| React | 18 | UI framework |
| Vite | 5 | Build tool / dev server |
| Tailwind CSS | 3 | Utility-first styling |
| Recharts | 2 | Charts (Line, Bar, Scatter, Area) |
| PapaParse | 5 | CSV parsing |
| Lucide React | 0.383 | Icons |

---

## 🔧 Troubleshooting

| Problem | Solution |
|---|---|
| `npm install` fails | Try `npm install --legacy-peer-deps` |
| Blank page / no data | Check browser console; ensure CSVs are in `public/data/` |
| Chart not rendering | Ensure `tea_score` values are numeric in your CSV |
| Upload not working | Make sure you're uploading `.csv` files (not `.xlsx`) |
| Slow initial load | Normal — 3 CSV files (~4MB total) are parsed client-side |

---

## 📝 Notes

- All data processing is **100% client-side** — no backend, no server, no API.
- The app works completely offline once loaded.
- `usr_username` is the join key — students with the same username across years are treated as the same person.
- Students present in only 1 year are still shown but have no improvement score.
