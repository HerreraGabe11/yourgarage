# Your Garage — Setup & Deploy Guide

## Project Structure

```
my-garage/
├── index.html          ← The app
├── README.md           ← This file
└── models/             ← 3D model files (.glb)
    ├── 1995_ford_f350.glb
    ├── 2011_bmw_m3.glb
    └── 2013_chevrolet_corvette_z06.glb
```

## Adding 3D Models

The app auto-loads GLB files from the `models/` folder for default vehicles. To add or replace a model:

1. Place the `.glb` file in the `models/` folder
2. The filename must match what's in `DEFAULT_MODELS` inside `index.html`:

```js
var DEFAULT_MODELS = {
  'f350_default': 'models/1995_ford_f350.glb',
  'bmw_m3_default': 'models/2011_bmw_m3.glb',
  'corvette_default': 'models/2013_chevrolet_corvette_z06.glb'
};
```

To add a new bundled vehicle, add an entry to both the `defaults` array in `seedDefault()` and the `DEFAULT_MODELS` map.

### Where to find GLB models

- [Sketchfab](https://sketchfab.com) — search for car models, download as GLB
- [CGTrader](https://www.cgtrader.com) — free and paid models
- [TurboSquid](https://www.turbosquid.com) — search with "glTF" filter

## Deploy to Vercel

### 1. Install Git

Check if you have it:
```bash
git --version
```

If not:
- **Mac**: It will prompt you to install Xcode Command Line Tools — click Install
- **Windows**: Download from [git-scm.com](https://git-scm.com/download/win)

### 2. Create a GitHub repository

1. Go to [github.com](https://github.com) → sign up or log in
2. Click **+** → **New repository**
3. Name it `my-garage`
4. Leave it **Public**
5. Do NOT check "Add a README"
6. Click **Create repository**

### 3. Push to GitHub

Open your terminal in the `my-garage` folder:

```bash
cd my-garage
git init
git add .
git commit -m "Your Garage app with 3D models"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/my-garage.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

> **Note:** GitHub may ask for credentials. Use your username and a **Personal Access Token** (not your password). Create one at: GitHub → Settings → Developer Settings → Personal Access Tokens.

### 4. Deploy on Vercel

1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Click **"Add New Project"**
4. Find `my-garage` → click **Import**
5. Set Framework Preset to **"Other"**
6. Click **Deploy**

You'll get a URL like `https://my-garage-xxxxx.vercel.app`

### 5. Add to iPhone Home Screen

1. Open **Safari** on your iPhone
2. Go to your Vercel URL
3. Tap the **Share button** (square with arrow)
4. Tap **"Add to Home Screen"**
5. Name it "Your Garage" → tap **Add**

## Updating

After any changes:

```bash
git add .
git commit -m "describe your change"
git push
```

Vercel auto-deploys within ~30 seconds.

## How Bundled Models Work

- On first visit, the app fetches GLB files from `models/` and caches them in your browser's IndexedDB
- On future visits, models load instantly from the cache
- If you upload a custom model via the Edit modal, it overrides the bundled one (per device)
- Each device/browser caches independently — the first load fetches from the server, then it's cached
