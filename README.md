# IXL Buddy

A modern, feature-packed userscript for IXL. 

Honestly the most advanced IXL script out right now tbh. Most tools for the site are either completely abandoned or fall apart the second a question includes a geometry diagram, a fraction box, or a multi-part prompt. IXL Buddy was built from scratch to handle all of it cleanly. A good chunk of it was straight-up vibecoded late at night, but under the hood it's properly engineered with real DOM parsing, vision extraction, and zero-token math engines.

---

## What it does

### Multimodal AI Auto-Answer
Powered by Google Gemini (optimized for **Gemini 3.8 Flash** and **3.7 Flash**). It doesn't just guess text:
- **Every question type**: Handles standard multiple choice, multi-select ("select all that apply"), numeric/text input boxes, fractions, inline dropdowns, and interactive token/word selectors.
- **Vision & diagram extraction**: Automatically parses rendered SVG coordinate geometry and problem diagrams so the model actually sees what the question looks like.
- **Wrong-choice memory**: If an answer ever misses, it remembers the failed choice for that question ID and won't pick it again.

### Instant Programmatic Solvers
For common geometry and algebra patterns (transversal angles, triangle angle sum theorems, exterior angles, etc.), IXL Buddy doesn't even need to call the AI. It calculates the exact answer locally in under 5 milliseconds with zero API latency and zero token usage.

### Ambient GPU Dark Mode
A dedicated, high-contrast dark theme designed specifically for IXL's practice pages and student dashboard:
- Built with the native View Transitions API so you get smooth GPU crossfades between questions instead of getting flashbanged with bright white every time a new problem renders.
- Subtle ambient cyan and periwinkle glow accents that blend into the practice article.

### Stealth Mode & Custom Hotkeys
Keep your screen clean whenever you want. You can program any key chord (like `Alt + H`) or multi-key sequence to instantly hide or reveal the Buddy HUD.

### Session Analytics & Streak Counter
A live HUD dashboard that tracks your session stats in real time:
- Correct / incorrect breakdown
- Live accuracy percentage
- Current streak and all-time best streak
- Total questions solved programmatically vs. via AI
- Gemini API token and call counters

### Utilities
- **Copy Question**: Cleanly exports sanitized question text, LaTeX/MathJax expressions, and choices directly to your clipboard and developer console with one click.
- **Page Lock**: Intercepts accidental tab flips, unwanted redirects, and navigation away from your current practice skill.

---

## Installation

1. **Install a Userscript Manager**  
   Get [Tampermonkey](https://www.tampermonkey.net/) or [Violentmonkey](https://violentmonkey.github.io/) for Chrome, Edge, Firefox, or Brave.

2. **Add the Script**  
   Install `buddy.js` directly from your userscript manager or load it manually into your extensions dashboard.

3. **Get a Gemini API Key** *(Free)*  
   To use the AI Auto-Answer feature:
   - Go to [Google AI Studio](https://aistudio.google.com/apikey) and click **Create API Key**.
   - Open any IXL practice page (`ixl.com`).
   - Click the **Gear (⚙)** icon on the Buddy HUD to open **Settings**.
   - Paste your key into the **Gemini API Key** field and hit **Save Key**.

4. **Start Practicing**  
   Toggle on **Auto Answer** or **Copy Question** on the main panel and you're good to go.

---

## Settings & Customization

The settings menu includes dedicated sections for:
- **Theme & Appearance**: Instant toggle between standard Light mode and ambient Dark mode, with live status indicators and GPU fading.
- **AI Intelligence**: Key validation with real-time connection status (`CONNECTED`, `CONFIGURED`, `NOT SET`), a show/hide key toggle, and a model picker (Gemini 3.8 Flash, 3.7 Flash, 3.1 Pro, etc.).
- **Shortcuts**: Interactive hotkey recorder to set your hide/show key combination.
- **Usage & Metrics**: Full stat cards with a protected reset button (requires a 3-second safety confirmation).

---

## Under the Hood

- **Zero External Dependencies**: Pure vanilla JavaScript running directly inside the userscript sandbox.
- **CSP Workaround**: Bypasses strict Content Security Policies on IXL using background `GM_xmlhttpRequest` calls and object blob URLs for asset rendering.
- **Reactive DOM Scanners**: Uses targeted `MutationObserver` loops to detect new questions, answer validation checkmarks, and dynamic page state without polling or slowing down your browser.

---

## Disclaimer

IXL Buddy is an independent open-source project intended for educational assistance, study verification, and workflow research. Please use responsibly.
