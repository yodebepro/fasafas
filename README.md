# FasaFas — Video Meeting Platform

A full-featured video meeting platform built with HTML, CSS, and JavaScript. Powered by **Jitsi Meet** for real video calls.

---

## How to Upload to GitHub (Keep All Folders Intact)

### Method 1 — GitHub Desktop (Easiest)

1. Download **[GitHub Desktop](https://desktop.github.com/)** and install it
2. Log in with your GitHub account
3. Click **File → New Repository**
   - Name: `fasafas`
   - Local Path: the folder where you extracted this ZIP
   - Click **Create Repository**
4. Click **Commit to main** (bottom left)
5. Click **Publish repository** (top right)
6. Done — all folders uploaded correctly

### Method 2 — Git Command Line

Open Terminal in the extracted folder:

```bash
git init
git branch -M main
git add .
git commit -m "Initial FasaFas upload"
git remote add origin https://github.com/YOUR_USERNAME/fasafas.git
git push -u origin main
```

### Method 3 — GitHub Web Upload

1. Create a new repo at github.com
2. Click "uploading an existing file"
3. Open your extracted fasafas folder, select ALL (Ctrl+A), drag into GitHub
4. Commit changes

---

## Enable GitHub Pages

1. Repo → Settings → Pages
2. Source → **GitHub Actions**
3. Auto-deploys via `.github/workflows/deploy.yml`
4. Live URL: `https://YOUR_USERNAME.github.io/fasafas/`

---

## Project Structure

```
fasafas/
├── index.html                ← Public home page
├── .github/workflows/        ← Auto GitHub Pages deploy
├── admin/index.html          ← Admin portal (secret URL)
├── backend/index.html        ← Dev portal (secret URL)
├── assets/css/               ← Stylesheets
├── assets/js/translate.js    ← 100+ language translation
└── pages/
    ├── prejoin.html          ← Camera/mic test
    ├── meeting-room.html     ← Real video calls (Jitsi)
    └── features/             ← 5 features x 11 sub-pages each
```

---

## Secret Portals

| Portal | URL | User | Password |
|--------|-----|------|----------|
| Admin | /admin | admin@fasafas.com | FasaFas@2026 |
| Backend | /backend | dev@fasafas.com | Backend@2026 |

---

## Video Calls

Uses **Jitsi Meet** (free, no account needed):
- Real camera and microphone
- Screen sharing, chat, hand raise
- 100+ participants supported
