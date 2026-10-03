# AttendX — Smart College Attendance

> **Know your attendance. Plan your semester.**

AttendX is a responsive web app prototype for college attendance. Students see where they stand and plan ahead. Faculty mark attendance for their scheduled classes. It runs entirely in the browser with mock data, and is structured so a real backend can be connected later.

**Live demo:** `https://your-attendx.onrender.com` *(update after deploying)*

---

## Demo Credentials

Pick a profile type on the sign-in page, then enter the matching credentials (or click **Use these** on the landing page).

| Profile | User ID    | Password     |
| ------- | ---------- | ------------ |
| Student | `21CSE001` | `student123` |
| Faculty | `FAC101`   | `faculty123` |

> These credentials are for the demo only. Login is checked in the browser and is **not secure**. Replace it with real authentication before using real student data.

---

## Features

### Student dashboard
- Student name, semester, and section
- Overall attendance ring with the 75% requirement
- Subject-wise cards: classes attended, total classes, percentage, and status
  - 🟢 Safe (80% or above)
  - 🟡 At Risk (75% to 79%)
  - 🔴 Shortage (below 75%)
- Attendance insights calculated from the data, such as how many classes can be missed or must be attended to reach 75%
- Weekly attendance trend chart
- Recent attendance with Present/Absent status
- **What if? simulator**: enter upcoming classes and classes you expect to attend to see projected attendance
  `(attended + future attended) / (total classes + future classes)`

### Faculty dashboard
- Today's scheduled classes from the mock timetable
- **Mark Attendance** opens the student roster with Present/Absent toggles
- **Mark All Present** and **Save Attendance**
- Saving updates the shared data, so the student dashboard reflects the change
- Weekly timetable view

---

## Demo Flow

**Student:** Sign in → view attendance → spot a subject shortage → use the simulator → see projected attendance.

**Faculty:** Sign in → view today's classes → mark students Present/Absent → save.

**See the update:** After saving as faculty, click **Log out**, sign in as the student, and the dashboard shows the new numbers. Data lives in memory, so don't refresh the page between steps.

---

## Project Structure

```
attendx/
├── index.html   # The whole app (HTML, CSS, JavaScript in one file)
└── README.md
```

No build tools or dependencies. The only external resource is the Google Fonts stylesheet.

---

## Run Locally

```bash
# Python
python -m http.server 8000

# or Node
npx serve .
```

Open `http://localhost:8000`. You can also double-click `index.html`.

---

## Deploy on Render (Static Site)

1. Push `index.html` and `README.md` to a GitHub repository (the file must be named `index.html` and sit in the repo root).
2. In [Render](https://dashboard.render.com), click **New +** → **Static Site** and select the repository.
3. Use these settings:

| Setting               | Value             |
| --------------------- | ----------------- |
| **Branch**            | `main`            |
| **Build Command**     | *(leave blank)*   |
| **Publish Directory** | `.`               |
| **Environment Variables** | none          |

4. Click **Create Static Site**. Render gives you a public `onrender.com` URL, and every push to `main` redeploys automatically.

---

## Customizing the Mock Data

Everything lives in the `DB` object near the top of the script in `index.html`:

| Key                | What it controls                                   |
| ------------------ | -------------------------------------------------- |
| `DB.users`         | Demo login IDs and passwords                       |
| `DB.student`       | Student name, roll number, semester, section       |
| `DB.faculty`       | Faculty name and department                        |
| `DB.subjects`      | Attended (`a`) and total (`t`) classes per subject |
| `DB.roster`        | Students shown to faculty when marking attendance  |
| `DB.timetable`     | Weekly schedule (time and subject per day)         |
| `DB.trend`         | Weekly attendance percentages for the chart        |
| `DB.required`      | Required attendance percentage (default 75)        |

---

## Connecting a Real Backend Later

All data access goes through the `API` object in the script. Replace each function body with a `fetch()` call and the interface needs no changes.

| Function               | Suggested endpoint                  |
| ---------------------- | ----------------------------------- |
| `API.login`            | `POST /auth/login`                  |
| `API.getStudent`       | `GET /students/me`                  |
| `API.getSubjects`      | `GET /students/me/attendance`       |
| `API.getRecent`        | `GET /students/me/attendance/recent`|
| `API.getTrend`         | `GET /students/me/attendance/trend` |
| `API.getRoster`        | `GET /sections/:id/students`        |
| `API.getTimetable`     | `GET /timetable`                    |
| `API.saveAttendance`   | `POST /attendance`                  |

---

## Known Limitations

- Data resets when the page is refreshed (no database yet).
- Login is a client-side mock with no real security or sessions.
- The mock backend only tracks the demo student's attendance counts.
- On weekends, the faculty dashboard shows Monday's timetable.

---

## Roadmap

- Real authentication and a database
- Timetable sync from a college system
- Attendance reports and exports

---

## License

MIT — free to use and modify.
