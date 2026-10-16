# IXL Buddy

[![Install](https://img.shields.io/badge/Install-Userscript-blue.svg)](https://raw.githubusercontent.com/prototbh/IXL-Buddy/main/ixl-buddy.user.js)

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

## Installation & How to Run

You can run IXL Buddy either as a persistent userscript (recommended) or directly in your browser's developer console (no extensions needed).

### Option 1: Run via Browser Console (No Extension Needed)
*Ideal for school Chromebooks or locked-down computers where browser extensions cannot be installed.*

1. Open any practice skill or dashboard page on [IXL](https://www.ixl.com).
2. Press `F12` (or `Ctrl+Shift+J` on Windows/Chromebook, `Cmd+Option+J` on Mac) to open DevTools.
3. Open [`ixl-buddy-console.js`](ixl-buddy-console.js), select all (`Ctrl+A`), copy (`Ctrl+C`), paste it into the **Console** tab, and press **Enter**.
4. The Buddy HUD will appear in the bottom-right corner immediately!

> [!NOTE]
> **Need to bypass Content Security Policy (CSP)?**  
> Because IXL sends a strict CSP header that restricts raw console scripts from loading external API models or assets, you may need to apply a 1-minute DevTools override if you run into network blocks. Follow the instructions below.

#### DevTools CSP Bypass Instructions (For Console Users)

1. Open DevTools (`F12`) on IXL and navigate to the **Sources** tab.
2. In the left sidebar, click the **`>>`** overflow button and select **Overrides**.
3. Click **Select folder for overrides**, choose any empty folder on your computer, and click **Allow** when your browser prompts for permission. (This creates a local override folder accessible to DevTools for this bypass).
4. Switch to the **Network** tab in DevTools, reload the page, and scroll all the way to the top to locate the very first request (the `dashboard` or skill page request).
5. Click this first request, switch to the **Headers** tab, and scroll down slightly until you see **Response Headers**.
6. Find the `content-security-policy` header (the long list of allowed URLs and directives). Hover over it and click the **pencil (✏️) icon**.
7. Select the entire value, delete it, and type in:
   ```text
   allow all
   ```
   *(This overrides IXL's restrictive CSP header to let IXL Buddy's assets and Gemini API requests connect smoothly).*
8. **Keep DevTools Open**: Do not close the DevTools window during your practice session, as closing DevTools will disable the override.
9. **Test the Bypass**: Switch to the **Console** tab and run this quick verification snippet:
   ```javascript
   fetch("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.21/lodash.min.js")
     .then(res => res.text())
     .then(code => {
       eval(code);
       console.log("%c[CSP BYPASS SUCCESS]%c lodash loaded: " + typeof _, "color: green; font-weight: bold;", "color: reset;");
     })
     .catch(err => console.error("[CSP BLOCKED]", err));
   ```
10. If you see `%c[CSP BYPASS SUCCESS]`, you're all set! Paste and run [`ixl-buddy-console.js`](ixl-buddy-console.js). If you get hit with a wall of red error text, re-check that the override folder has permission and that the header change was saved.

---

### Option 2: Run via Userscript Manager (Persistent)
*Automatically runs on every IXL page load without touching the console or DevTools.*

[![Install](https://img.shields.io/badge/Install-Userscript-blue.svg)](https://raw.githubusercontent.com/prototbh/IXL-Buddy/main/ixl-buddy.user.js)

1. Install [Tampermonkey](https://www.tampermonkey.net/) or [Violentmonkey](https://violentmonkey.github.io/) in your browser.
2. Click the badge above or install [`buddy.js`](buddy.js) in your manager.
3. Open any IXL page and the Buddy icon will load automatically.

---

### Getting an AI Key (Free)
To use the Auto-Answer feature:
1. Grab a free API key at [Google AI Studio](https://aistudio.google.com/apikey).
2. Open any IXL practice page with Buddy running.
3. Click the **Gear (⚙)** icon on the Buddy HUD to open **Settings**.
4. Paste your key into the **Gemini API Key** field and hit **Save Key**.
5. Toggle on **Auto Answer** on the main cheats panel and let Buddy go to work.

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
