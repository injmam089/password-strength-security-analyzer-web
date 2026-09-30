# Password Strength & Security Analyzer — Web Edition

> A privacy-first, fully client-side password analysis and secure password generation tool built with HTML5, CSS3, and vanilla JavaScript.
>
> Password analysis happens in the browser, while password generation uses the Web Crypto API (`crypto.getRandomValues()`). No application backend is required.

[![Client-Side](https://img.shields.io/badge/Architecture-Client--Side-0f172a?style=flat-square)](#-security--privacy)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](#-tech-stack)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)](#-tech-stack)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?style=flat-square&logo=javascript&logoColor=111827)](#-tech-stack)
[![Web Crypto API](https://img.shields.io/badge/Web%20Crypto-API-16a34a?style=flat-square)](#-security--privacy)

## 🌐 Project Overview

**Password Strength & Security Analyzer — Web Edition** is a browser-based cybersecurity utility for:

- analyzing password strength in real time;
- checking common-password exposure against an offline dictionary;
- explaining password characteristics through transparent heuristic scoring;
- providing contextual security recommendations; and
- generating random passwords with the browser's Web Crypto API.

The application is intentionally built without a custom backend, database, framework, or build step. It can be served as a static website from platforms such as Vercel, Netlify, or GitHub Pages.

## ✨ Key Features

### 🔍 Password Analyzer

- Real-time password analysis while typing or pasting.
- Strength score from **0–100**.
- Three classifications: **WEAK**, **MEDIUM**, and **STRONG**.
- Circular SVG score gauge.
- Character-length and character-category analysis.
- Uppercase, lowercase, number, and special-character checks.
- Offline common-password dictionary detection.
- Entropy-category display for educational feedback.
- Contextual recommendations for improving password hygiene.
- Clear and show/hide controls for the password input.

### 🎲 Secure Password Generator

- Uses `crypto.getRandomValues()` through the Web Crypto API.
- Uses rejection sampling when selecting random indexes to avoid modulo bias.
- Guarantees representation of every selected character category.
- Uses a cryptographically secure Fisher–Yates shuffle.
- Configurable password length from **8 to 64 characters**.
- Character categories:
  - Uppercase letters
  - Lowercase letters
  - Numbers
  - Special characters
- Quick length presets.
- Copy-to-clipboard support.
- Show/hide generated password.
- Direct **Analyze Generated** workflow.

## 🛡️ Security & Privacy

The application is designed to keep password values inside the browser during normal operation.

### 1. No Application Backend

There is no custom server application, database, or API endpoint for password analysis. The analysis and generation logic runs in JavaScript loaded by the browser.

### 2. Passwords Are Not Sent by the Application

The application code does not submit password values to a backend, analytics service, or third-party API. The only network request in the application code is the browser `fetch()` used to load the local `data/common_passwords.txt` dictionary when it is available.

> **Important:** Serving the website still requires the browser to download the application's static files. The privacy claim is specifically that the **password value itself is not transmitted by the application**.

### 3. No Application Password Persistence

The application does not use:

- `localStorage`
- `sessionStorage`
- IndexedDB
- cookies for credential storage

Passwords remain in the active page state while the user is interacting with the application.

### 4. Cryptographically Secure Randomness

Password generation uses the browser's Web Crypto API rather than `Math.random()`.

The generator:

1. obtains random 32-bit values through `crypto.getRandomValues()`;
2. applies rejection sampling when selecting random indexes;
3. guarantees selected character-category requirements; and
4. performs a secure Fisher–Yates shuffle using the same cryptographic random source.

The application does **not** claim that the browser's randomness is specifically hardware-backed; the implementation relies on the browser/platform's Web Crypto implementation.

### 5. Local Common-Password Dictionary

The application includes `data/common_passwords.txt` and loads it into an in-memory `Set` for fast lookup. A small embedded fallback dictionary is also included so common-password detection can continue to work when the external dictionary cannot be fetched, such as some `file://` scenarios.

## 🔄 How It Works

### Password Analysis

```text
User enters password
        │
        ▼
Client-side JavaScript analysis
        │
        ├── Length check
        ├── Uppercase check
        ├── Lowercase check
        ├── Number check
        ├── Special-character check
        ├── Common-password lookup
        └── Entropy category calculation
        │
        ▼
Heuristic score (0–100)
        │
        ▼
WEAK / MEDIUM / STRONG
        │
        ▼
Security recommendations
```

### Password Generation

```text
User selects length + character categories
        │
        ▼
Validate configuration
        │
        ▼
crypto.getRandomValues()
        │
        ├── Select one character from each active category
        ├── Fill remaining positions from combined pool
        └── Cryptographically secure Fisher–Yates shuffle
        │
        ▼
Validate generated password
        │
        ▼
Generated credential
```

## 🧮 Strength Scoring Model

The analyzer uses a **transparent educational heuristic**, not a password-cracking simulator or a guarantee of security.

| Component | Contribution |
|---|---:|
| Length `< 6` | 0 points |
| Length `6–7` | +10 points |
| Length `8–11` | +25 points |
| Length `12–15` | +35 points |
| Length `16+` | +45 points |
| Each character category | +10 points |
| All 4 categories | +15 bonus |
| 3 categories | +5 bonus |
| ≤1 category with length ≥6 | −15 penalty |
| Common password | Score capped at 10 |
| Length `< 6` | Score capped at 20 |

### Classification

| Score | Classification |
|---:|---|
| 0–39 | WEAK |
| 40–69 | MEDIUM |
| 70–100 | STRONG |

> The score is an educational heuristic. A `STRONG` result does not guarantee that a password is resistant to every attack, including phishing, credential stuffing, targeted guessing, or password reuse attacks.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| HTML5 | Semantic application structure |
| CSS3 | Responsive interface and visual design |
| Vanilla JavaScript | Application logic and UI interactions |
| Web Crypto API | Cryptographically secure random password generation |
| SVG | Password-strength gauge and interface icons |
| Python HTTP Server | Simple local development server |
| Node.js | Optional local static serving / development support |
| Vercel | Optional static deployment |
| Netlify | Optional static deployment |

## 📁 Project Structure

```text
password-strength-security-analyzer-web/
├── assets/
│   └── shield.svg                  # Application favicon / security icon
├── css/
│   └── style.css                   # Responsive UI styling
├── data/
│   └── common_passwords.txt        # Offline common-password dictionary
├── js/
│   ├── app.js                      # UI controller and event handling
│   ├── passwordChecker.js          # Password analysis and scoring engine
│   └── passwordGenerator.js        # Secure password generation engine
├── index.html                      # Main application page
├── netlify.toml                    # Netlify security headers
├── vercel.json                     # Vercel security headers
└── README.md                       # Project documentation
```

## 🚀 Run Locally

No dependency installation is required for the browser application itself.

### Option 1 — Python Built-in Server

From the repository root:

```bash
python -m http.server 8000
```

Open:

```text
http://localhost:8000
```

### Option 2 — Node.js Static Server

If Node.js is installed:

```bash
npx serve .
```

Or:

```bash
npx http-server .
```

### Option 3 — Open Directly

You can also open `index.html` directly in a modern browser.

However, serving the project through a local HTTP server is recommended because the browser may restrict `fetch()` requests to `data/common_passwords.txt` when the page is opened using the `file://` protocol. The application contains an embedded fallback dictionary for this case.

## 🌐 Deployment

This project has no build step. Deploy the **repository root** as a static site.

### Vercel

1. Import the GitHub repository into Vercel.
2. Keep the project root as the repository root.
3. No build command is required.
4. Deploy.

The included `vercel.json` adds security-related response headers.

### Netlify

1. Import the GitHub repository into Netlify, or upload the project directory.
2. Use the repository root as the publish directory.
3. No build command is required.
4. Deploy.

The included `netlify.toml` configures security-related response headers.

### GitHub Pages

The project is already a static website, so GitHub Pages can serve the repository directly.

1. Open **Settings → Pages** in the repository.
2. Select **Deploy from a branch**.
3. Select the desired branch.
4. Select the repository root (`/`) as the folder.
5. Save the configuration.

## 🔒 Security Headers

The project includes security-header configuration for Vercel and Netlify.

| Header | Purpose |
|---|---|
| `Content-Security-Policy` | Restricts permitted resource sources |
| `X-Content-Type-Options: nosniff` | Helps prevent MIME-type sniffing |
| `X-Frame-Options: DENY` | Prevents framing by other pages |
| `Referrer-Policy: no-referrer` | Limits referrer information |
| `Permissions-Policy` | Disables unnecessary browser capabilities |

The CSP is configured to allow resources from the application's own origin while permitting the inline styles required by the current interface.

## 🧪 Verification & Manual Testing

The current repository does **not** include the previously documented `scratch/test_web_suite.js` automated test suite. Therefore, that test command has intentionally been removed from this README rather than documenting a file that is not present.

Recommended manual verification:

- Enter short and long passwords.
- Test uppercase, lowercase, number, and special-character combinations.
- Test passwords from `data/common_passwords.txt`.
- Verify that common passwords receive the intended score cap.
- Generate passwords at the minimum and maximum lengths.
- Enable and disable individual character categories.
- Verify that every selected category is represented in generated passwords.
- Test copy-to-clipboard functionality.
- Test the Analyze Generated workflow.
- Test the application on desktop and mobile layouts.
- Inspect browser storage and confirm that the application does not write passwords to Web Storage or cookies.
- Inspect network requests and confirm that password values are not submitted by the application.

## 📱 Browser Compatibility

The application is intended for modern browsers that support standard ES6+ JavaScript, SVG, Clipboard APIs, and the Web Crypto API.

| Browser | Support |
|---|---|
| Google Chrome / Chromium | Modern versions |
| Microsoft Edge | Modern versions |
| Mozilla Firefox | Modern versions |
| Apple Safari | Modern versions |
| Android Chrome | Modern versions |
| iOS Safari | Modern versions |

> Browser support depends on the availability and behavior of the underlying Web APIs. Very old browser versions are not a target for this project.

## 📸 Screenshots

### Main Dashboard

![Password Strength & Security Analyzer](Screenshot%202026-09-30%20195121.png)

The main dashboard combines real-time password analysis with the secure password generator.

### Password Generator

![Password Generator](Screenshot%202026-09-30%20194222.png)

The generator supports configurable password length, character categories, secure generation, clipboard copying, and entropy estimation.

### Password Analyzer

![Password Analyzer](Screenshot%202026-09-30%20194123.png)

The analyzer provides real-time strength scoring, password criteria checks, entropy classification, character diversity analysis, and common-password detection.
## ⚠️ Security Disclaimer

This project is an educational password-strength and password-generation tool. Its strength score is a transparent heuristic and should not be interpreted as a formal security guarantee.

For real-world account security:

- use unique passwords for different services;
- prefer long, randomly generated passwords or passphrases;
- consider using a reputable password manager;
- enable multi-factor authentication where available; and
- never enter sensitive credentials into tools you do not trust.

## 🚧 Future Improvements

Potential future improvements include:

- [ ] Add automated browser-based tests.
- [ ] Add a larger and versioned common-password dataset.
- [ ] Add automated accessibility testing.
- [ ] Add more detailed password-pattern detection.
- [ ] Add configurable scoring profiles.
- [ ] Add a dedicated test suite for the Web Crypto generator.
- [ ] Add screenshot-based documentation and a live demo link.

## 🤝 Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch:

```bash
git checkout -b feature/your-feature
```

3. Make your changes.
4. Test the application locally.
5. Commit your changes:

```bash
git add .
git commit -m "Add: your feature"
```

6. Push the branch:

```bash
git push origin feature/your-feature
```

7. Open a Pull Request.

## 👨‍💻 Author

**Injmam**

- GitHub: [@injmam089](https://github.com/injmam089)
- Repository: [password-strength-security-analyzer-web](https://github.com/injmam089/password-strength-security-analyzer-web)

## 📄 License

If this project is intended to be open source, add the license you want to use (for example, MIT) as a `LICENSE` file in the repository and update this section accordingly.

> **Current repository note:** No `LICENSE` file was present in the supplied project archive, so this README does not claim a specific license.
