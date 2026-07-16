# Nudgement 🔍 <img src="nudgement-extension/icons/128.png" align="right" width="48" height="48">

[![Chrome Extension](https://img.shields.io/badge/Chrome-Extension-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)](https://chrome.google.com/webstore)
[![Manifest V3](https://img.shields.io/badge/Manifest-V3-green?style=for-the-badge)](https://developer.chrome.com/docs/extensions/mv3/intro/)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-yellow?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

**Nudgement** is a Chrome extension that shows you how your online content is gradually nudging your attention and worldview over time. It tracks what kinds of content you consume across 8 topic dimensions and builds a personal exposure profile from your browsing.

---

## 🧠 Why Nudgement?

Every page you visit is shaped by someone's intent: to provoke, to sell, to alarm, or to entertain. Over time, a steady diet of one kind of content quietly shifts what feels normal, urgent, or true. Nudgement makes that drift visible. Not to judge what you read, but to show you the shape of it.

---

## 🌟 Key Features

* 📊 **Nudgemeter:** Scores the current page across 8 topic dimensions (Outrage, Politics, Health, Finance, Consumerism, AI & Tech, Productivity, Entertainment) and shows you which ones dominate.
* 🗓️ **7-Day Diet:** Tracks your content history locally and surfaces how your exposure has been distributed over the past week.
* 🔍 **Nudge Signals:** Highlights the specific phrases that triggered the analysis with plain-language explanations of what they do.
* 🔒 **Local and Private:** All analysis happens on your device using pattern matching. Nothing leaves your browser.

---

## 🛠️ How It Works

1. **Extraction:** `extractor.js` pulls the main readable text from any page using four fallback strategies: targeted DOM selectors, Mozilla Readability, adaptive ranked candidates, and a clean body fallback. Surface type is detected automatically (article, video, social, page).

2. **Scoring:** `scorer.js` runs 14 tactic signal patterns (clickbait, fear appeal, outrage, false urgency, etc.) against the extracted text. Those signals are then mapped to 8 topic dimensions using keyword detection. A page with fear-appeal signals and health keywords scores high on Health. The same signals on a finance article score high on Finance.

3. **History:** Every analysis result is appended to a local store in `chrome.storage.local`. The popup reads the past 7 days to build your weekly exposure profile. Capped at 500 entries, oldest removed first.

4. **Badge:** The extension badge shows the Nudgemeter score (0-100) for the current page, colour-coded green / amber / red.

---

## 📐 Nudgemeter Dimensions

| Dimension | What it reflects |
|---|---|
| **Outrage** | Content relying on anger, moral indignation, or scandal framing |
| **Politics** | Politically coded content regardless of leaning |
| **Health** | Health anxiety, wellness claims, medical fear content |
| **Finance** | Market fear, economic anxiety, money urgency |
| **Consumerism** | Shopping pressure, FOMO, product promotion |
| **AI & Tech** | Tech hype cycles, AI doomism, startup culture |
| **Productivity** | Self-optimisation pressure, hustle culture, life hacking |
| **Entertainment** | Celebrity gossip, viral content, pop culture |

---

## ⚡ Installation

1. **Clone or download** this repository
2. **Open Extensions:** Navigate to `chrome://extensions/` in Chrome
3. **Enable Developer Mode:** Toggle the switch in the top-right corner
4. **Load Unpacked:** Click **Load unpacked** and select the `nudgement-extension/` folder
5. **Start Browsing:** Visit any webpage and click the extension icon

---

## 🧪 Running Tests

```bash
npm test
```

Tests cover `nudgemeter_score` ranges and `nudge_profile` dimension outputs across 15 content scenarios including neutral articles, political outrage, health fear appeals, finance urgency, AI hype, and entertainment clickbait.

---

## 🖥️ Optional Backend

A minimal Flask backend is included in `backend/` for local experimentation. It mirrors the JS scoring logic and supports opt-in history storage.

```bash
cd backend
pip install -r requirements.txt
python app.py
```

The backend is completely optional. The extension runs fully offline without it.

> [!NOTE]
> `torch` and `transformers` have been removed from the backend requirements. They were listed as future dependencies in the original hackathon build but were never used. The current scorer is regex-based. Future ML improvements should use WASM-based models (ONNX Runtime Web, TensorFlow.js) that run inside the browser extension itself, keeping it local-first with no heavy installs.

---

## 📁 Project Structure

```
nudgement-extension/     ← load this as an unpacked extension
  manifest.json
  Readability.js         ← Mozilla Readability (bundled)
  extractor.js           ← text extraction pipeline
  scorer.js              ← tactic signals + dimension scoring
  content.js             ← injected into every page
  background.js          ← service worker, cache, history
  popup.html / css / js  ← extension popup UI

backend/                 ← optional Flask backend
  app.py
  requirements.txt

tests/
  eval_cases.json        ← 15 scored content scenarios
  scoring.test.js        ← scorer unit tests
  extractor.test.js      ← extractor unit tests
```

---

## 🤝 Contributing

Issues and PRs are welcome. The scoring patterns in `scorer.js` are the easiest entry point: both the 14 tactic signal patterns and the topic keyword lists for each dimension can be improved without touching anything else in the codebase. Keep `backend/app.py` in sync with `scorer.js` when changing signal weights or adding new patterns.

---

> [!NOTE]
> **A note on the name:** This project was originally built as **Boundier** at an IIT Bombay hackathon. That name moved to a separate project (an autonomous Discord bot). This project was then briefly renamed **Cognitive Firewall** as a placeholder. It is now **Nudgement**, which better reflects what it actually does.
