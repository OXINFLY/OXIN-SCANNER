# 🛡️ OXIN SCANNER (Android IP Diagnostic Utility)

**OXIN SCANNER** is a premium, robust, hyper-optimized Android network diagnostic utility designed to scan, test, and filter clean IP addresses. It performs rigorous multi-layer validation checks, bypassing firewalls and spoofing middleware to pinpoint stable network entry points with millisecond accuracy.

The application has been meticulously redesigned to deliver ultra-fast scans, zero-lag performance on low-end hardware (e.g., Samsung Galaxy A12), and modern, responsive cyber-themed interfaces built completely on **Jetpack Compose** and **Material Design 3**.

---

## 🎯 Key Capabilities

*   **⚡ Concurrency Engine**: Employs Kotlin Coroutines, Kotlin Flow, and a custom Semaphore structure to run up to 45 parallel IP tests concurrently without CPU throttling.
*   **🛠️ 3-Layer Advanced Validation**: Completely defeats Middlebox spoofing, proxy caching, and routing redirects by asserting network integrity across three layers (TCP, TLS/SNI, HTTP/1.1).
*   **📱 Low-End Device Optimization**: Tailor-made rendering mechanics built to keep the application butter-smooth even on budget octacore processors (3GB RAM).
*   **🌐 Real-Time Interactive Log Console**: Emits color-coded feedback for Clean, Scanning, and Failed IP diagnoses dynamically.

---

## 🏗️ Technical Architecture

The app conforms strictly to **Clean Architecture** patterns under the **MVVM (Model-View-ViewModel)** paradigm to separate concerns and preserve lifecycle integrity during dynamic network scans:

```
                  ┌─────────────────────────────────────────┐
                  │              MainActivity               │  ◄── (Jetpack Compose Screen)
                  └────────────────────┬────────────────────┘
                                       │
                         Observes StateFlows / Flows
                                       │
                                       ▼
                  ┌─────────────────────────────────────────┐
                  │           DashboardViewModel            │  ◄── (State and Batch Pipeline)
                  └────────────────────┬────────────────────┘
                                       │
                             Launches Coroutines
                                       │
                                       ▼
                  ┌─────────────────────────────────────────┐
                  │                IpTester                 │  ◄── (Socket Core / Engine)
                  └─────────────────────────────────────────┘
```

1.  **View Layer (`MainActivity.kt`)**: Implements declarative composables that consume states from the ViewModel using `collectAsState()`. Uses specialized performance layout modifiers.
2.  **State/Pipeline Layer (`DashboardViewModel.kt`)**: Manages inputs, validation configurations, asynchronous job pooling, and state publishing. Intercepts incoming logs in a thread-safe Queue to batch-update UI state streams.
3.  **Network Engine (`IpTester.kt`)**: Implements native socket-level network operations directly on Kotlin JVM thread pipelines.

---

## 📑 Multi-Layer Test Verification Pipeline

Traditional ping/TCP checks are easily fooled by transparent proxies or active firewalls that spoof handshakes. OXIN Scanner employs an exhaustive **3-Layer verification process** to guarantee absolute end-to-end IP cleanliness:

```
               [ IP Address Target ]
                        │
                        ▼
      ┌───────────────────────────────────┐
  1   │             Layer 4               │   TCP 3-Way Handshake
      │          (TCP Connect)            │   (Establish raw channel to port 443)
      └─────────────────┬─────────────────┘
                        │  (Success)
                        ▼
      ┌───────────────────────────────────┐
  2   │             Layer 6               │   Cryptographic TLS Handshake
      │          (SSL / SNI)              │   (Wraps Socket & injects custom SNI headers)
      └─────────────────┬─────────────────┘
                        │  (Success)
                        ▼
      ┌───────────────────────────────────┐
  3   │             Layer 7               │   Application HTTP Verification
      │       (HTTP HEAD Protocol)        │   (Infiltrates payload; parses headers)
      └─────────────────┬─────────────────┘
                        │  (HTTP Code 200/3XX/4XX Validated)
                        ▼
               ✨ [ CLEAN IP ] ✨
```

### 🔹 Layer 4: TCP Handshake (`InetSocketAddress`)
Initiates a standard raw socket connection towards the target IP on port `443` (HTTPS). This step verifies routing availability and initial TCP packet exchange latency.

### 🔹 Layer 6: TLS/SSL Custom Negotiation (`SSLSocket`)
Securely wraps the raw connected socket using `SSLSocketFactory`. It manually injects customized **SNI (Server Name Indication) Host Headers** configured by the user into the TLS client-hello packet. It triggers `sslSocket.startHandshake()` to complete cryptographic validation. This level identifies and discards IPs blocked on the TLS handshake tier.

### 🔹 Layer 7: HTTP Payload Handshake Verification (`HTTP HEAD`)
Sends an explicit custom HTTP plain header over the established secure link:
```http
HEAD / HTTP/1.1
Host: <your-sni-hostname>
User-Agent: OXIN-Scanner/2.5 (Android)
Accept: */*
Connection: close
```
Parses the incoming HTTP protocol status line directly through the socket stream. An IP is proclaimed **Clean** ONLY if it successfully returns an HTTP-compliant status line structure (e.g., `200 OK`, `302 Found`, etc.). This isolates and destroys mock routing endpoints, ISP manipulation, and payload-drop configurations.

---

## ⚡ Low-End Device Smoothness Blueprint

Low-tier chipsets (like the Samsung Galaxy A12 using MediaTek Helio P35) struggle severely when Jetpack Compose initiates rapid recompositions (re-rendering the UI layout dozens of times per second). We've engineered special bypasses to support 60FPS rendering on constraints:

1.  **Thread-Safe Concurrency Pooling**: Logs are captured in a non-blocking `ConcurrentLinkedQueue`. Instead of immediately updating the Compose state for every log (which creates UI thread lockouts), a specialized coroutine flushes background logs to the UI in batches every **350ms**.
2.  **LazyColumn Rendering Limiters**: 
    *   The raw real-time console logs list is capped inside `MainActivity` using `takeLast(30)` to drop legacy logs from memory.
    *   The active scan progress list is capped using `take(25)` to render only the most relevant real-time mutations.
3.  **Explicit Structural Stable Keys**: Both list layouts now leverage explicit state keys (`key = { it.ip }` and `key = { it }`) inside LazyColumns. This prevents Jetpack Compose from re-drawing off-screen items when other items change.
4.  **Stable State Remembers**: Costly computational operations inside layout cycles (such as determining `isClean` or `isFailed` status strings) are wrapped inside Compose `remember(logMessage)` blocks, keeping UI performance exceptionally fast.

---

## 🎨 Visual Identity & Dynamic Icons

*   **Modern Theme**: Employs an ultra-premium Dark Cyberpunk UI with high-contrast glowing copper oranges and neon teal highlights.
*   **Custom Launcher Icon**: Preloaded with a highly detailed application launcher vector icon combining cloud infrastructure, radar scanning rings, a protective metallic shield, and a bold chrome 'X'.
*   **Adaptive Theme Support**: Configured inside `AndroidManifest.xml` via circular adaptive XML layers (`ic_launcher.xml` & `ic_launcher_round.xml`) mapping to unified dark backdrop assets.
