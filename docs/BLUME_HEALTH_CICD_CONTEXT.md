# Blume Health Mobile App: CI/CD Architecture & Technical Implementation Context
> **Exportable Context Document** — Copy and paste this directly to another AI or use it as an architectural reference.

---

## 1. Project Background & System Identity
- **Repository / App:** `blume_health_flutter_app`
- **Platform:** Cross-platform Flutter mobile client (iOS + Android) serving patients in a HIPAA-compliant digital health, telehealth, and chronic care ecosystem.
- **Tech Stack:** Flutter 3.8+ / Dart 3, `flutter_bloc` 9.0, `dio` 5.7, `go_router` 16.0, `flutter_secure_storage` 10.0, Auth0, Sentry 9.25, Shorebird 2.0.
- **Branching Model:** Strict **Trunk-Based Development** on `main`. Every merge to `main` auto-builds a dev APK. Releases are promoted systematically: `dev` → `staging` → `production`.

---

## 2. The Problem: Legacy Setup & Why We Migrated Away From Xcode Cloud
Initially, the project used **Xcode Cloud** for iOS builds alongside an out-of-band pipeline for Android. This legacy setup introduced major pain points:
1. **Platform Fragmentation:** iOS and Android releases ran in isolated, disjointed silos. There was no single pane of glass to trigger, coordinate, or track synchronized dual-platform releases.
2. **Manual Versioning Chaos:** App version bumps and changelog tracking were done manually or in an ad-hoc fashion, risking version drift between platforms and store rejections.
3. **Shorebird Engine Incompatibility:** 
   - Shorebird Code Push requires building with the Shorebird Flutter engine and registering the exact compiled release binary hash (`shorebird release ios`).
   - When Xcode Cloud ran a subsequent out-of-band archive, it compiled with standard Xcode toolchains, creating a differing binary hash.
   - This mismatched binary hash caused Shorebird over-the-air (OTA) patches to fail on user devices.
4. **Limited Notification & Governance:** Xcode Cloud lacked native webhook hooks to integrate custom AWS Lambda + Slack alerting, Sentry symbol management, and automated Google Play/App Store Connect state polling.

---

## 3. The Solution: Centralized Trunk-Based Pipeline on GitHub Actions

### A. Automated Semantic Versioning via Google `release-please` (`release-please.yml`)
- Operates on conventional commits (`feat:`, `fix:`, `chore:`, `perf:`).
- Maintains a persistent, auto-updating release PR titled: `chore: release X.Y.Z`.
- When an engineer merges the `chore: release X.Y.Z` PR into `main`:
  1. Automatically bumps `versionName` (`X.Y.Z`) in `pubspec.yaml`.
  2. Generates and updates `CHANGELOG.md` with conventional commit categorization.
  3. Creates a formal **GitHub Release** and cuts the Git tag `vX.Y.Z`.
  4. Automatically dispatches both `android-build-production.yml` and `ios-build-production.yml` in parallel.

### B. Decoupled Versioning & Build Provenance
- **Decoupled Architecture:** The marketing semver `versionName` (`X.Y.Z`) is governed by `release-please`, while the monotonic store `buildNumber` (`versionCode` on Android, `CFBundleVersion` on iOS) is computed dynamically in CI as:
  ```bash
  git rev-list --count HEAD
  ```
- **Build-Time Injections (`--dart-define`):**
  Every production/staging build injects traceability parameters directly into Flutter:
  - `APP_GIT_SHA=$(git rev-parse HEAD)`: Injected into HTTP headers (`X-App-Commit`) and Sentry tags (`app.commit`) to trace every backend request and client error back to the exact commit SHA.
  - `APP_BUILD=$(git rev-list --count HEAD)`
  - `APP_ENV=<production|staging|dev>`
  - `SENTRY_RELEASE=blume.mobile@X.Y.Z+<build>`

### C. Fully Automated Dual-Store Submissions
1. **Android Production (`android-build-production.yml`):**
   - Compiles release `.aab` bundle with ProGuard/R8 obfuscation.
   - Uploads ProGuard mapping files and native symbols to Sentry.
   - Deploys directly to the Google Play Console `production` track with 100% rollout behavior.
   - Triggers an AWS Lambda notifier to poll Google Play approval status and post compact updates to `#blume-mobile-app` in Slack.
2. **iOS Production (`ios-build-production.yml`):**
   - Runs on macOS GitHub Actions runners (`macos-14`).
   - Authenticates with Apple via App Store Connect API keys (`APP_STORE_CONNECT_KEY_ID`, `APP_STORE_CONNECT_ISSUER_ID`).
   - Compiles and codesigns the release `.ipa`.
   - Uploads `.ipa` to App Store Connect, waits for Apple validation processing, submits for App Review, and sets automatic release after Apple approval.
   - Extracts and uploads dSYM DWARF symbols to Sentry.
   - Arms the Slack release-review notifier.

---

## 4. Emergency Over-The-Air (OTA) Patching via Shorebird
To protect patient continuity in a healthcare app (e.g., if a regression blocks daily insulin or symptom logging), we cannot wait 24–48 hours for App Store re-review.
- **Patch Workflows:**
  - `shorebird-ios-production-patch.yml`
  - `shorebird-android-production-patch.yml`
- **Execution Mechanism:**
  - Dispatched via manual `workflow_dispatch` from `main` against an exact Shorebird-recorded release.
  - Recompiles only the modified Dart AOT bytecode into a compact binary diff.
  - Pushes the patch to Shorebird servers.
  - Active patient devices download the patch silently in the background and boot into the patched code on next app start within **5 minutes**, completely bypassing Google Play and Apple App Store review cycles.

---

## 5. Summary Matrix for Quick Reference

| Feature | Legacy Setup (Xcode Cloud) | New Modernized Setup (GitHub Actions) |
|---|---|---|
| **Control Plane** | Fragmented (Xcode Cloud for iOS, out-of-band for Android) | **Centralized single pane of glass** in `.github/workflows/` |
| **Versioning** | Manual `pubspec.yaml` edits | **Automated SemVer** via Google `release-please` |
| **Changelog** | Manual notes | **Auto-generated `CHANGELOG.md`** from Conventional Commits |
| **Build Numbers** | Hardcoded or manual increments | **Computed in CI** via `git rev-list --count HEAD` |
| **Shorebird OTA** | Incompatible (differing binary hashes broke patches) | **Fully integrated** (`shorebird release` & `shorebird patch`) |
| **Store Submission** | Manual clicking in consoles | **100% automated** submit to Play Store & App Store Review |
| **Build Tracing** | Limited | `--dart-define` injects `APP_GIT_SHA`, `APP_BUILD`, `SENTRY_RELEASE` |
| **Alerting** | Default emails | **State-change Slack Pulse notifications** via AWS Lambda polling |
