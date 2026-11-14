# Mobile Developer Technical Interview Guide (Flutter • Swift • iOS • Security)

A comprehensive, interactive, and visually styled technical interview guide covering:
- **Swift & iOS Fundamentals**: Value vs Reference types, SwiftUI vs UIKit, Swift Concurrency (async/await, actors, TaskGroup), Image downsampling & caching, Property wrappers, Delegate/NotificationCenter/UserDefaults, Singleton pattern, Multiple environment API setups.
- **Flutter & Dart Core**: Single-threaded event loop & Isolates, Widget & App Lifecycle, Networking (Dio/http), 90K items pagination with scroll preservation, Crashlytics, Flutter vs Jetpack Compose, APK reduction, Build flavors, MethodChannel & Pigeon.
- **State Management & DI**: BLoC vs Cubit, get_it & injectable, Riverpod, Provider.
- **Security & Fintech**: SSL/TLS Certificate Pinning, End-to-End (E2E) API payload encryption (AES-GCM + RSA), Card/PAN security & PCI-DSS, Root & Jailbreak detection, Android Keystore & iOS Keychain, BLE GATT architecture.
- **Architecture**: MVVM with Repository pattern, OS migration (Android 15 → 16), Java to Kotlin/Flutter migration.
- **Live Coding Challenges**:
  - Asynchronous Armstrong Number finder with 2-second delay in both **Dart** and **Swift**.
  - JSON user grouping by city with average age calculation in both **Dart** and **Swift**.
  - Bonus interview coding patterns (Two Sum, Debounce, Memoization, Palindrome, Duplicates).

---

## 🚀 Free Hosting & Deployment Options

### 1. GitHub Pages (Recommended — 100% Free)
The web application is already structured inside the `/docs` folder, which is the native GitHub Pages publishing source.

1. Create a repository on GitHub (e.g. `Deloitte-QA-Sample` or `mobile-interview-prep`).
2. Run these commands locally:
   ```bash
   git add .
   git commit -m "Add interview prep app"
   git remote add origin https://github.com/<YOUR_USERNAME>/<REPO_NAME>.git
   git branch -M main
   git push -u origin main
   ```
3. Go to your repo on GitHub:
   - **Settings** → **Pages** (in left sidebar)
   - Under **Build and deployment** > **Branch**:
     - Branch: `main`
     - Folder: `/docs`
   - Click **Save**.
4. In ~1 minute, your site will be live at:
   `https://<YOUR_USERNAME>.github.io/<REPO_NAME>/`

---

### 2. Vercel (Free & Instant)
- Install CLI: `npx vercel`
- Or import your GitHub repo directly on [vercel.com](https://vercel.com) (set Root Directory to `docs` or project root).

---

### 3. Netlify (Free Drag-and-Drop)
- Simply drag and drop the `docs` folder onto [Netlify Drop](https://app.netlify.com/drop).
- Instant live URL with custom domain support.

---

### 4. Cloudflare Pages (Free Unlimited Bandwidth)
- Link your GitHub repo to Cloudflare Pages.
- Set build output directory to `docs`.
