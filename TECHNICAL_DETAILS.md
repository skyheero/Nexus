# MQTT Chat Client — Browser-Only Real-Time Messaging Demo

A lightweight real-time chat client built to explore **MQTT in the browser**, **UI responsiveness with Web Workers**, and **client-side encryption**.
The application runs entirely in the browser and connects to a private coordination backend and a public MQTT transport layer over secure WebSocket.

**Tech stack:** Svelte, Web Workers, Rust (WebAssembly), MQTT over WSS, AES-GCM

---

## 🔗 Live Demo

👉 [Nexus - secure-chat](https://skyheero.github.io/Nexus/)

> Note: The demo uses a private coordination service. Access may be limited, but the UI and flow are publicly visible.

---

## 🎯 Motivation

This project was built to experiment with:

* using **MQTT instead of typical REST/WebSocket APIs** for real-time apps
* keeping the UI responsive by isolating networking and crypto in **Web Workers**
* implementing **end-to-end message encryption in the browser**
* integrating **Rust-based WebAssembly** into a modern frontend stack

It also serves as a technical foundation for future multiplayer or board-game style web applications.

---

## 🧭 User Flow

1. **Login** — enter display name and client ID (or use generated defaults)
2. **Loading** — establishes MQTT session inside a Web Worker
3. **Chat** — real-time messaging with online user list and unread indicators

---

## 🏗 Architecture (High Level)

* **Client-only application**

  * Deployed as static assets via GitHub Pages or any HTTPS static host
* **Frontend**

  * Built with **Svelte** for lightweight reactive UI
* **Networking**

  * MQTT over secure WebSocket (**WSS**)
* **Worker + WASM**

  * MQTT client runs inside a **Web Worker** to avoid blocking the main thread
  * Core protocol and crypto logic implemented in **Rust and compiled to WebAssembly**
  * UI and worker communicate via structured message passing
* **Keep-alive & status**

  * Periodic pings maintain session liveness and emit connection status events to the UI

---

## 🌍 Backend Connectivity Strategy

To minimize cost and operational complexity, the system separates **message transport** from **application coordination**:

* **Public MQTT broker (EMQX)**

  * Used only as a secure transport layer over WSS
  * Provides global reach, TLS, and scalable fan-out
  * No application secrets or business logic are stored on the broker

* **Private coordination backend (SBC, Armbian)**

  * Lightweight service that only performs minimal user coordination and key provisioning
  * Not exposed to the public internet; no public IP is required
  * Acts as a control plane rather than a message relay

This design allows users worldwide to communicate securely while keeping the private server hidden and operating at very low cost.

---

## 🔐 Security and Privacy

* **Client-side encryption**

  * All message payloads are encrypted using **AES-GCM** before transport
* **Authenticated topics**

  * Topic names are derived using keyed hashes to prevent unauthorized subscription
* **Secrets not included**

  * Broker endpoints, keys, and derivation secrets are not present in this repository or build artifacts
* **No persistence**

  * Messages and user state exist only in memory during the session

---

## 📊 Performance & Compatibility

**Browser Support:**
- Chrome/Edge 90+
- Firefox 88+
- Safari 15+

**Bundle Characteristics:**
- WASM module: ~40KB (compressed)
- Initial load: < 2 seconds on 3G
- Message latency: < 100ms (typical)

---

## 🧪 What This Project Demonstrates

* Real-time messaging using **MQTT in web browsers**
* Performance-oriented frontend design using **Web Workers**
* Practical **Rust → WebAssembly integration** in production-style builds
* Secure client-side cryptography with **authenticated encryption**
* Cost-efficient system design using public transport infrastructure and private control services
* Static-site deployment with private real-time backends

---

## 🌐 Deployment

* Hosted via **GitHub Pages** as static assets
* Any HTTPS static host or CDN can serve the same build output

---
