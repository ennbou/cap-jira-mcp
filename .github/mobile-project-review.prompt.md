## **description: "Comprehensive Mobile Project Quality Assessment (Flutter, iOS, Android)" applyTo: "\*\*/\*.{dart,swift,kt,java,xml,json,yaml}" tools: \['codebase', 'fetch', 'context7'\]**

# **Mobile Project Review Playbook (Flutter, iOS, Android)**

This prompt guides an AI assistant through a **comprehensive, deterministic review** of a Mobile application project. It adapts its analysis based on the detected technology stack (Flutter/Dart, Native iOS/Swift, or Native Android/Kotlin). It analyzes project structure, UI performance, architecture, state management, and platform best practices.

## **Task 0\. Stack Detection & Structure Analysis**

Start When: Prompt is invoked.  
Objective: Identify the mobile framework and map the project structure.  
Steps:

1. Analyze the project root using \#codebase.  
2. **Detect Stack:**  
   * **Flutter:** Look for pubspec.yaml, lib/main.dart, ios/Runner, android/app.  
   * **Native iOS:** Look for \*.xcodeproj, \*.xcworkspace, Podfile, Package.swift.  
   * **Native Android:** Look for build.gradle (app module), AndroidManifest.xml, src/main/java|kotlin.  
3. **Scan Directory Structure:**  
   * **Source:** (e.g., lib/ for Flutter, App/Sources for iOS, src/main for Android).  
   * **Resources:** Assets, images, fonts, localization files (.arb, .strings, res/values).  
   * **Config:** Build variants, flavors, schemes, environment configs.  
4. **Inventory Layers:**  
   * UI / Presentation (Widgets, Views, ViewControllers, Composables)  
   * State Management (BLoC, Providers, ViewModels, Reducers)  
   * Domain / Business Logic (UseCases, Interactors)  
   * Data / Repository (API Services, Local Storage, DTOs)  
   * Navigation logic  
5. **Count Metrics:** Classes/Files, Lines of Code (LOC), Asset size.

**End Criteria:** Stack identified (Flutter vs iOS vs Android) and structure mapped.

## **Task 1\. Architecture & Design Analysis**

Objective: Evaluate mobile-specific architecture quality.  
Steps:

1. **Architecture Pattern:**  
   * Verify usage of standard patterns: **Clean Architecture**, **MVVM**, **MVP**, or **BLoC** (Flutter).  
   * Check for "Massive View Controller" or "God Widget" anti-patterns.  
   * Ensure UI code is decoupled from Business Logic.  
2. **Navigation & Routing:**  
   * specific check for type-safe routing.  
   * Deep linking handling.  
   * Separation of navigation logic from UI components.  
3. **Dependency Injection:**  
   * **Flutter:** GetIt, Injectable, Riverpod, Provider.  
   * **Android:** Hilt, Koin, Dagger.  
   * **iOS:** Swinject, or native Dependency Injection via initializers.  
   * *Rule:* Dependencies should be injected, not instantiated inside classes.  
4. **Directory/Package Organization:**  
   * Feature-first structure (recommended) vs Layer-first structure.  
   * Logical grouping of files.

**End Criteria:** Architecture assessment with stack-specific findings.

## **Task 2\. Detailed Quality Checklist Review**

## **🌍 GLOBAL RULES (Platform-Agnostic)**

*Applies to all codebases.*

### **2.1 Naming & Style**

**Checklist:**

* Consistent casing (PascalCase for Classes, camelCase for vars/methods).  
* File naming matches platform conventions (snake\_case for Dart, PascalCase for Swift/Kotlin).  
* No magic numbers or hardcoded strings (use Localization/Constants).  
* Boolean variables properly prefixed (is, has, should).

### **2.2 Code Quality & Maintainability**

**Checklist:**

* **SOLID Principles** adhered to.  
* **DRY:** Extracted reusable widgets/views.  
* **Complexity:** Max indentation level 3-4. Methods \< 40 lines.  
* **Comments:** Documentation for public APIs, distinct from "what the code is doing."

### **2.3 Error Handling & Logging**

**Checklist:**

* **No Silent Failures:** catch (e) {} is forbidden.  
* **User Feedback:** Errors propagate to UI (Toasts, Snackbars, Alerts) generic error states.  
* **Logging:** Use standard loggers (Timber, OSLog, Logger) instead of print().  
* **Crash Reporting:** Integration of Crashlytics or Sentry hooks.

## **📱 MOBILE-SPECIFIC RULES (Stack-Dependent)**

*Apply ONLY the section matching the detected stack.*

### **2.4 🐦 FLUTTER SPECIFIC RULES (If Stack \== Flutter)**

**Checklist:**

* **Build Method Purity:** build() methods must be pure, side-effect free, and fast. No async calls or logic inside build.  
* **Widget Splitting:** Large build methods split into smaller StatelessWidget classes (NOT helper methods returning Widgets).  
* **Const Usage:** const constructors used wherever possible to reduce GC pressure.  
* **State Management:** \- No setState in complex screens.  
  * Logic moved to BLoC/Cubit/Provider/Controller.  
  * BuildContext used safely across async gaps (mounted check).  
* **Performance:**  
  * Use ListView.builder for long lists (not ListView with children).  
  * Avoid Opacity widget with animations (use AnimatedOpacity).  
  * Images cached (cached\_network\_image).  
* **Null Safety:** Strict null safety utilized; no \! bang operator usage unless absolutely guaranteed.  
* **Platform Channels:** Type-safe implementation if Native code is used.

### **2.5 🤖 ANDROID SPECIFIC RULES (If Stack \== Kotlin/Java)**

**Checklist:**

* **Coroutines:** Correct scope usage (viewModelScope, lifecycleScope). No GlobalScope.  
* **Jetpack Compose:** \- Composable functions strictly side-effect free.  
  * remember and LaunchedEffect used correctly.  
  * State hoisting applied (State flows down, events flow up).  
* **XML (Legacy):** ViewBinding or DataBinding used instead of findViewById.  
* **Lifecycle:** Observers clean up correctly. No memory leaks with Context (Application vs Activity Context).  
* **Threading:** Main thread strictly for UI updates. IO operations on Dispatchers.IO.  
* **Serialization:** usage of Kotlin Serialization or Moshi over Gson.

### **2.6 🍎 iOS SPECIFIC RULES (If Stack \== Swift)**

**Checklist:**

* **Memory Management:** Check for Retain Cycles. Use \[weak self\] in closures.  
* **SwiftUI:**  
  * @StateObject vs @ObservedObject usage distinctions.  
  * Views are small and composable.  
  * MainActor usage for UI updates.  
* **UIKit (Legacy):** Delegate pattern implementation uses weak var.  
* **Concurrency:** Async/Await preferred over completion handlers. GCD usage checked for main thread blocking.  
* **Force Unwrapping:** No \! force unwrapping optionals. Use if let, guard let.  
* **Extensions:** Extensions used to organize protocol conformances.

## **Task 3\. Security & Data Privacy**

Objective: Identify mobile security risks.  
Steps:

1. **Permissions:** Review Info.plist / AndroidManifest.xml. Are permissions minimal and justified?  
2. **Data Storage:** \- Sensitive data (Tokens) in **Keychain** (iOS) or **EncryptedSharedPreferences** (Android).  
   * NEVER in UserDefaults or SharedPreferences plain text.  
3. **Network:** \- SSL Pinning implementation check.  
   * No HTTP traffic (Cleartext traffic disabled).  
4. **Obfuscation:** ProGuard/R8 rules (Android) setup.  
5. **API Keys:** Not hardcoded in git-tracked files.

## **Task 4\. Performance & UX Assessment**

Objective: Evaluate responsiveness and resource usage.  
Steps:

1. **Jank Detection:** Identify heavy computations on the Main/UI thread.  
2. **Image Assets:** Check for oversized assets (e.g., loading 4MB image for a generic avatar).  
3. **App Size:** Unused resources or heavy dependencies.  
4. **Offline Mode:** Logic exists for handling no connectivity.  
5. **Accessibility:** \- **Flutter:** Semantics widgets used?  
   * **iOS:** VoiceOver labels.  
   * **Android:** contentDescription.

## **Task 6\. Scoring & Final Assessment**

**Objective:** Convert findings into a score (0-100).

| Criterion | Weight | Type |
| :---- | :---- | :---- |
| **GLOBAL** | **40** |  |
| Naming & Style | 5 | Global |
| Code Quality & Maintainability | 10 | Global |
| Error Handling | 10 | Global |
| Security | 15 | Global |
| **MOBILE SPECIFIC** | **60** |  |
| Architecture & Layers | 15 | Mobile |
| State Management | 15 | Mobile |
| UI Performance (Jank/Builds) | 10 | Mobile |
| Memory & Lifecycle | 10 | Mobile |
| Platform Best Practices | 10 | Mobile |
| **Total** | **100** |  |

**Score Interpretation:**

* **90-100:** App Store Ready / Featured Quality  
* **75-89:** Solid, Minor Polish Needed  
* **60-74:** Functional but needs Refactoring  
* **\<60:** Critical Issues / Not Release Ready

## **Task 7\. Generate Project Review Report**

**Objective:** Produce comprehensive markdown report.

### **Report Structure:**

\# Mobile Project Quality Review

\*\*Project Type:\*\* \[Flutter / iOS Native / Android Native\]  
\*\*Date:\*\* \[Date\]  
\*\*Score:\*\* X/100

\#\# Executive Summary  
\[Summary of app health, architecture choice, and stability\]

\#\# Critical Issues (Blockers)  
1\. \*\*\[Issue\]\*\* \- \[File:Line\]  
   \- Why: \[Explanation\]  
   \- Fix: \[Code Snippet\]

\#\# Mobile Architecture Analysis  
\- \*\*Pattern:\*\* \[e.g., MVVM with Riverpod\]  
\- \*\*Navigation:\*\* \[Analysis\]  
\- \*\*Dependency Injection:\*\* \[Analysis\]

\#\# Detailed Findings (By Category)  
...

\#\# Performance & Resources  
\- \*\*App Size Risk:\*\* \[Low/High\]  
\- \*\*Main Thread Blocking:\*\* \[Detected/Clear\]  
\- \*\*Asset Optimization:\*\* \[Status\]

\#\# Security Audit  
\- \*\*Permissions:\*\* \[Analysis\]  
\- \*\*Storage:\*\* \[Analysis\]  
\- \*\*Network:\*\* \[Analysis\]

## **Task 9\. Generate Action Items List**

Objective: Create prioritized task list for remediation.  
Format: Same as Task 9 in the Java prompt (Critical/High/Medium/Low).

## **Execution Notes**

1. **Context is King:** If reviewing a Prototype, be lenient on Testing. If Production, be strict on Error Handling.  
2. **State Management:** Be opinionated based on the *chosen* solution (don't suggest Riverpod if they are using BLoC, unless BLoC is implemented wrongly).  
3. **UI vs Logic:** Heavily penalize business logic inside UI Views/Widgets.

## **Termination Rule**

Do NOT produce the report until stack detection is confirmed and specific platform rules are applied.