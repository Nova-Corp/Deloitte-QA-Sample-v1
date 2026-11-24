# Mobile Developer Technical Interview Guide (Flutter • Swift • iOS • Security)

A comprehensive, interactive, and visually styled technical interview guide covering:
- **Swift & iOS Fundamentals**: Value vs Reference types, SwiftUI vs UIKit, Swift Concurrency (async/await, actors, TaskGroup), Image downsampling & caching, Property wrappers, Delegate/NotificationCenter/UserDefaults, Singleton pattern, Multiple environment API setups.
- **Flutter & Dart Core**: Single-threaded event loop & Isolates, Widget & App Lifecycle, Networking (Dio/http), 90K items pagination with scroll preservation, Crashlytics, Flutter vs Jetpack Compose, APK reduction, Build flavors, MethodChannel & Pigeon.
- **State Management & DI**: BLoC vs Cubit, get_it & injectable, Riverpod, Provider, Dagger/Hilt vs Flutter DI.
- **Security & Fintech**: SSL/TLS Certificate Pinning, End-to-End (E2E) API payload encryption (AES-GCM + RSA), Card/PAN security & PCI-DSS, Root & Jailbreak detection, Android Keystore & iOS Keychain, BLE GATT architecture.
- **Architecture**: MVVM with Repository pattern, OS migration (Android 15 → 16), Java to Kotlin/Flutter migration, How to explain your project (STAR Framework).
- **Live Coding Challenges**:
  - Asynchronous Armstrong Number finder with 2-second delay in both **Dart** and **Swift**.
  - JSON user grouping by city with average age calculation in both **Dart** and **Swift**.
  - Bonus interview coding patterns (Two Sum, Debounce, Memoization, Palindrome, Duplicates).
- **🌟 High-Yield Senior Questions (Flutter & iOS)**:
  - *Flutter*: Memory leaks detection & DevTools profiling, Offline-First sync architecture, App startup time (TTFF) & Impeller shader compilation, Custom RenderBox & Box Constraints protocol, Enterprise testing pyramid & Golden tests.
  - *Swift/iOS*: Automatic Reference Counting (ARC) in-depth, retain cycles, weak vs unowned, autoreleasepool, Swift 6 Data Race Safety & Sendable, Coordinator Pattern & VIPER architecture, SwiftData vs Core Data vs Keychain, Push notifications lifecycle & Notification Service Extensions.
- **🎯 Round 2 & Technical Lead Deep-Dive (New Section)**:
  - Complete guide in [ROUND_2_INTERVIEW_ANALYSIS.md](file:///Users/sparkout/Documents/Deloitte-QA-Sample/docs/ROUND_2_INTERVIEW_ANALYSIS.md) and interactive tab in [docs/index.html](file:///Users/sparkout/Documents/Deloitte-QA-Sample/docs/index.html).
  - Exportable Blume Health CI/CD Context in [BLUME_HEALTH_CICD_CONTEXT.md](file:///Users/sparkout/Documents/Deloitte-QA-Sample/docs/BLUME_HEALTH_CICD_CONTEXT.md).
  - Covers Zero-Downtime SSL Pinning backup rotation, RSA math & AES-GCM hybrid encryption, NPCI UPI Common Library (CL) SDK, MVVM Clean Architecture end-to-end login flow, Keychain & Secure Enclave, Swift Concurrency multi-API orchestration, Threading/Deadlock scenarios, Crashlytics dSYM internals, BLoC vs Provider vs Riverpod, Platform channel debugging & Dart FFI, iOS performance profiling, XCUITest Page Object pattern, Fastlane CI/CD & Xcode Cloud migration to GitHub Actions, SOLID & ARC, and real-world production incident response.

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
