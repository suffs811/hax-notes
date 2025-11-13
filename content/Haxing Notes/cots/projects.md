## 🧰 CLI / TUI Projects

### 1. **“Task Ranger” — A TUI Task Manager**
- **Description:** Build a terminal-based to-do/task manager with categories, due dates, and search.
- **Tech/Concepts:**
    - Use [`Bubbletea`](https://github.com/charmbracelet/bubbletea) for the TUI.
    - Store data locally in JSON or SQLite via `sqlx`.
    - Add export/import and color themes for flair.
- **Challenge upgrade:** Sync tasks via a lightweight Go REST API.
---
### 2. **Git Log Visualizer**
- **Description:** A CLI tool that reads `git log` data and displays an ASCII graph of commits and branches.
- **Tech/Concepts:**
    - Parsing command outputs using `os/exec`.
    - Handling graph structures in Go.
    - Fun ASCII rendering practice.
## 🌐 Full Stack / Web Projects
### 3. **“BookLog” — Personal Reading Tracker**
- **Description:** Track books you’re reading, ratings, notes, and progress.
- **Stack:**
    - Backend: Go + `gin` or `echo`.
    - DB: PostgreSQL with `gorm`.
    - Frontend: Svelte or React.
- **Challenge upgrade:** Add public sharing pages and book cover scraping from an API (e.g. Open Library).    
---
### 4. **Real-Time Chat App**
- **Description:** Build a simple web-based chat with WebSockets in Go.
- **Stack:**
    - Go `gorilla/websocket` for real-time connections.
    - Basic HTML/JS frontend (no need for a full framework).
    - Practice concurrency and message broadcasting.
- **Challenge upgrade:** Add private rooms and persistent chat history.
---
### 5. **“GoBoard” — Kanban App**
- **Description:** A minimalist Trello-like app with draggable cards.
- **Stack:**
    - Go + Fiber or Gin for API.
    - SQLite or PostgreSQL for data.
    - Frontend: Vue or React with drag-and-drop.
- **Learning angle:** Build clean REST endpoints and practice Go JSON handling.
