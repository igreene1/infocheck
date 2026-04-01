# InfoCheck - Threat Intelligence Platform

InfoCheck is a custom, browser-based web application I built for my Final Year Project. It serves as a 14-day longitudinal intervention comparing two training formats: a gamified "threat intelligence" simulation and a standard text-based manual. Both formats teach critical thinking skills for evaluating misinformation and conspiracy-style claims.

I built this project entirely from scratch using vanilla HTML, CSS, and JavaScript. I did not use external libraries or frameworks (like React or Bootstrap) to keep the application lightweight and entirely client-side.

## Tech Stack
* **HTML5:** Core page structure and text-control layouts.
* **CSS3:** Custom retro-terminal styling, UI layout, and keyframe animations.
* **Vanilla JavaScript (ES6):** State management, DOM manipulation, scoring logic, and local data persistence.

---

## Technical Implementation & Methods

I broke the application down into several functional components. Below is the breakdown of how I built each feature, the methods I used, and the documentation I referenced.

### 1. CRT "Retro Terminal" Aesthetics
* **What it does:** Gives the gamified version a dark, immersive hacker-terminal look, complete with screen glare and a moving scanner line. 
* **How I did it:** I used pure CSS. I applied a `linear-gradient` background to the `body::before` pseudo-element to simulate a curved glass screen reflection. For the moving green scanline, I created a fixed `div` (`.scanline`) spanning the width of the page and animated it using CSS `@keyframes` to continuously translate it from top to bottom.
* **Sources Used:** CodePen community snippets for "CSS CRT screen effects" and MDN Web Docs for CSS `@keyframes` syntax.

### 2. Single Page Application (SPA) Navigation
* **What it does:** Allows the user to switch between the Dashboard, Inbox, Leaderboard, and Service Record tabs instantly without reloading the browser window.
* **How I did it:** I wrapped each view in its own `div` container. I wrote a JavaScript function (`switchView`) that triggers on button clicks. This function loops through all view containers, applies a `.hidden` utility class (`display: none !important`) to hide them, and removes the class from the target view to display it.
* **Sources Used:** W3Schools "How To - Tabs" tutorial and MDN Web Docs for the `Element.classList` API.

### 3. Decryption Animation (Cognitive Friction)
* **What it does:** When a user opens an evidence file, the text scrambles rapidly for a brief period before revealing the actual text. This forces the user to slow down and adds weight to their decision to "decrypt" a file.
* **How I did it:** I implemented a JavaScript `setInterval` loop inside the `decryptFile` function. When triggered, the loop injects a random string of characters (`ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$`) into the DOM every 50 milliseconds. After a counter hits 12 iterations, I call `clearInterval()` and inject the actual evidence string from the array.
* **Sources Used:** Stack Overflow threads on creating a "JavaScript text scramble effect" and standard JS timing event documentation.

### 4. Dynamic Game Logic & Scenario Data
* **What it does:** Feeds the user 6 distinct scenarios, tracks their choices, and provides immediate customized feedback based on the verdict they select.
* **How I did it:** I hardcoded all scenario data (context, social media posts, evidence text, verdicts, and feedback) into a global JavaScript array of objects (`const cases`). A global variable (`currentDay`) tracks the user's progress. When the user selects a verdict, the `submitDecision` function updates their reputation score (`rep`) and dynamically injects the correct feedback text into the `#feedback-text` DOM element.
* **Sources Used:** FreeCodeCamp JavaScript data structure modules (specifically iterating through Objects and Arrays).

### 5. Leaderboard Memory & State Persistence
* **What it does:** Remembers the player's reputation score and their rival's (Sterling) score across sessions, making sure the leaderboard updates easily and does not reset if the user refreshes the page.
* **How I did it:** I utilized the browser's Web Storage API. The `initLeaderboard` and `updateLeaderboard` functions read and write to `localStorage`. Because `localStorage` only accepts strings, I use `JSON.stringify()` to save the leaderboard array and `JSON.parse()` to retrieve it. Before injecting the data into the HTML, I use the JavaScript `.sort()` method to rank the players from highest to lowest score.
* **Sources Used:** MDN Web Docs for `Window.localStorage` and JavaScript Array `.sort()` documentation.

### 6. Toast Notifications & Modals
* **What it does:** Delivers non-intrusive pop-up messages (like incoming emails or taunts from the rival) and full-screen achievement alerts when a user earns a medal.
* **How I did it:** For the toasts, I wrote a function (`showToast`) that uses `document.createElement('div')` to generate the popup, appends it to a fixed container, and uses a `setTimeout()` to remove it from the DOM after 5 seconds so they don't pile up. For the medals, I built a hidden modal overlay in the HTML and toggle its visibility using JS when the player selects a correct verdict.
* **Sources Used:** W3Schools "How To - Snackbar / Toast" and standard CSS flexbox layouts for centering modals.

---

## How to Run the Project
Because this application relies strictly on client-side vanilla code, no server or database setup is required to run it locally.

1. Clone or download the repository.
2. Open the `index.html` file directly in any modern web browser (Chrome, Firefox, Safari, Edge).
3. The game relies on `localStorage`. To wipe your progress and start a fresh session, open the browser's Developer Tools (F12), navigate to the Console, type `localStorage.clear()`, and refresh the page.
