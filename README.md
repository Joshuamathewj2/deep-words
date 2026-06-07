# Deep Words

> *A sanctuary for your thoughts*

**Deep Words** is a thoughtfully designed web application for private journaling, reflection, and problem-solving. Write freely in a distraction-free environment with mood tracking, a problem ledger for structured thinking, and seamless cloud synchronization.

**Live Demo:** [deep-words-psi.vercel.app](https://deep-words-psi.vercel.app)

---

## ✨ Features

### 📝 **Journaling**
- **Distraction-Free Editor**: Clean, minimal writing environment with no clutter
- **Mood Tracking**: Tag entries with moods (Reflect, Wonder, Vent, Gratitude)
- **Auto-Save**: Automatic saving with visual status indicator
- **Rich Statistics**: Real-time word count, character count, and line tracking
- **Entry Management**: Organize multiple entries with timestamps and previews

### 🎯 **Problem Ledger**
- **Structured Problem-Solving**: Identify problems, write descriptions, and plan solutions
- **Progress Tracking**: Mark problems as solved to visualize your progress
- **Expandable Details**: View full problem context with planned solutions
- **Chronological Organization**: Problems listed by creation date

### 🔐 **Privacy & Sync**
- **Secure Authentication**: Email-based authentication (Supabase)
- **Cloud Backup**: Automatic synchronization with Supabase database
- **Offline Support**: Local storage fallback for offline writing
- **Data Migration**: Automatic migration from local to cloud storage

### 🎨 **Design**
- **Dark Mode**: Eye-friendly, minimal dark theme
- **Responsive Layout**: Sidebar navigation with main editor area
- **Smooth Animations**: Polished interactions and transitions
- **Custom Typography**: Using Lora serif and JetBrains Mono fonts

---

## 🚀 Getting Started

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection (for cloud sync features)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Joshuamathewj2/deep-words.git
   cd deep-words
   ```

2. **Open in your browser:**
   - **Local development**: Open `index.html` directly in your browser
   - **Or use a local server** (recommended):
     ```bash
     # Python 3
     python -m http.server 8000
     
     # Or Node.js (http-server)
     npx http-server
     ```
   - Visit `http://localhost:8000`

### Configuration

The app uses **Supabase** for cloud storage. The Supabase credentials are configured in the HTML file:

```javascript
const SB_URL = 'https://egohjavpceluzozfambr.supabase.co';
const SB_KEY = 'sb_publishable_m61Rd7TFryL9oZZ967qiGQ_wgyZpNBN';
```

**To use your own Supabase backend:**
1. Create a Supabase account
2. Set up tables: `entries` and `problems`
3. Update the credentials in `index.html`

---

## 📱 Usage

### Creating an Entry
1. Click **"+ new entry"** in the sidebar
2. Add a title and write your thoughts
3. Select mood badges if desired (optional)
4. The entry auto-saves every 1.2 seconds

### Mood Tags
- **Reflect**: Introspection and self-analysis
- **Wonder**: Curiosity and exploration
- **Vent**: Expressing frustrations or emotions
- **Gratitude**: Appreciating positive moments

### Problem Ledger
1. Click **"Problems"** at the entry screen
2. Click **"+ add new problem"**
3. Fill in:
   - **Problem Title**: What's the issue?
   - **Problem Description**: Details and context
   - **Solution**: Your planned approach
4. Mark as solved when addressed

### Navigation
- **Selection Screen**: Choose between normal journaling or problem ledger
- **Sidebar**: View all entries, sorted by most recent first
- **Editor**: Write and manage your current entry
- **Bottom Bar**: Track word/character/line counts

---

## 🛠️ Technology Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML5, CSS3, Vanilla JavaScript |
| **Storage** | Supabase (PostgreSQL) + LocalStorage |
| **Authentication** | Supabase Auth (Email/Password) |
| **Fonts** | Google Fonts (Lora, JetBrains Mono) |
| **Deployment** | Vercel |

---

## 📁 Project Structure

```
deep-words/
├── index.html          # Complete single-file application
├── logo.png            # Application logo
└── README.md           # This file
```

The entire application is contained in a single `index.html` file, including:
- **CSS**: Custom dark theme styling
- **HTML**: Responsive layout structure
- **JavaScript**: All functionality (auth, journaling, sync, state management)

---

## 🔄 Data Flow

```
User Input
    ↓
JavaScript (Local Processing)
    ↓
Save Status Update
    ↓
Supabase Sync (if authenticated)
    ↓
Cloud Storage + LocalStorage
    ↓
Real-time UI Update
```

### Auto-Save Behavior
- **Triggered by**: Text input, mood selection, title changes
- **Debounce**: 1.2 seconds of inactivity
- **Status**: Visual indicator shows "saved" or "unsaved"
- **Offline**: Falls back to LocalStorage if Supabase unavailable

---

## 🔐 Security & Authentication

### Authentication Flow
1. **Splash Screen**: Loading animation
2. **Auth Screen**: Email/password entry
3. **Verification**: Supabase authentication
4. **Session Check**: Persistent session on return visits
5. **Logout**: Available via clicking logo in sidebar

### Access Control
- Currently configured for restricted access (`joshuamathewj2@gmail.com`)
- Easily modified in `handleAuth()` function for multi-user setup

### Data Privacy
- All data encrypted in transit (HTTPS)
- User-specific database rows with `user_id` isolation
- Local data never shared without explicit sync

---

## 📊 Database Schema

### `entries` Table
```sql
id (UUID)
user_id (UUID)
title (VARCHAR)
body (TEXT)
mood (VARCHAR) -- reflect, wonder, vent, gratitude
created_at (TIMESTAMP)
updated_at (TIMESTAMP)
```

### `problems` Table
```sql
id (UUID)
user_id (UUID)
title (VARCHAR)
description (TEXT)
solution (TEXT)
is_solved (BOOLEAN)
created_at (TIMESTAMP)
```

---

## 🎨 Customization

### Color Theme
Edit CSS variables in `<style>` section:
```css
:root {
  --bg: #0d0d0f;           /* Background */
  --accent: #b89ce8;       /* Primary accent (purple) */
  --gold: #d4a84b;         /* Secondary accent (gold) */
  --text: #e8e6e0;         /* Primary text */
  --text3: #5a5668;        /* Tertiary text (muted) */
}
```

### Fonts
- **Serif**: `Lora` (headings, body text)
- **Monospace**: `JetBrains Mono` (UI labels, code)

Edit Google Fonts link to change typefaces.

---

## 🐛 Troubleshooting

### Auth Screen Not Loading
- Check internet connection
- Clear browser cache and localStorage
- Open DevTools console for error messages

### Entries Not Syncing
- Verify Supabase credentials
- Check user authentication status
- Ensure database tables exist with correct schema

### Performance Issues
- Disable browser extensions
- Clear localStorage: `localStorage.clear()`
- Close other tabs/applications

### Lost Data
- Check browser LocalStorage (DevTools → Application → LocalStorage)
- Data persists even after logout in LocalStorage
- All cloud-synced data stored in Supabase

---

## 🔄 Browser LocalStorage

The app uses three LocalStorage keys:
- `deepwords_entries_v1` – Cached entries
- `deepwords_problems_v1` – Cached problems
- `deepwords_migrated_v1` – Migration flag

Clear with: `localStorage.clear()`

---

## 🚀 Deployment

### Deploy to Vercel
1. Push to GitHub
2. Connect repository to Vercel
3. Deploy (no build step required)
4. Update Supabase credentials if needed

### Deploy to Other Platforms
The single-file structure makes deployment simple:
- **Netlify**: Drag and drop `index.html`
- **GitHub Pages**: Push and enable in settings
- **Traditional Host**: Upload to web server

---

## 📝 License

This project is open source. Feel free to fork, modify, and use for personal projects.

---

## 👤 Author

**Joshua Mathew** — [GitHub](https://github.com/Joshuamathewj2)

---

## 💭 Philosophy

> *Deep Words* is built on the belief that reflection is essential to growth. By removing distractions and providing a space for unfiltered thought, we create conditions for genuine insight. Your thoughts deserve sanctuary.

---

## 📮 Support

Found a bug? Have a suggestion?  
Open an [issue](https://github.com/Joshuamathewj2/deep-words/issues) on GitHub.

---

**Last Updated**: June 7, 2026  
**Version**: 1.0.0
