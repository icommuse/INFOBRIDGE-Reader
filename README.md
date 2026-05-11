# INFOBRIDGE Reader
**The Secure Decoder for the INFOBRIDGE Ecosystem**

[![Open Live Preview](https://img.shields.io/badge/GitHub%20Pages-Open%20Live%20Preview-blue?style=for-the-badge&logo=github)](https://icommuse.github.io/INFOBRIDGE-Reader/index.html)
[![Download Latest ZIP](https://img.shields.io/badge/Release-Download%20Offline%20ZIP-green?style=for-the-badge&logo=archive)](https://github.com/icommuse/INFOBRIDGE-Reader/releases)

## 🌐 INFOBRIDGE Ecosystem

1.  **Encapsulation (iOS App)**: Converts data into **secure alphanumeric packets** (Base64 or AES-256 encrypted).
2.  **Transmission (QR Wedge)**: The scanner sends packets as raw keyboard input strings.
3.  **Restoration (Reader)**: The Reader reconstructs the packets into the original content or performs local decryption.

---

## 📖 About INFOBRIDGE Reader

INFOBRIDGE Reader is an offline-first web utility designed to reassemble and decode multi-part data streams transmitted via QR codes from the INFOBRIDGE iOS application. It serves secure data transfer across air-gapped systems.

> [!IMPORTANT]
> **Governance & Compliance**
> Usage within medical facilities, government offices, or secure corporate environments must strictly adhere to your organization's internal IT security policies. In most cases, coordination with and support from your **IT Administrator** is required to facilitate deployment, verify security standards, and ensure proper workstation configuration.

---

## 🚀 Quick Access (PWA & Live Preview)
You can test the Reader immediately or install it as a **Progressive Web App (PWA)** via the official GitHub Pages URL:
👉 **[https://icommuse.github.io/INFOBRIDGE-Reader/index.html](https://icommuse.github.io/INFOBRIDGE-Reader/index.html)**

> [!TIP]
> Accessing via this URL allows for instant **"Install App"** (Chrome/Edge) or **"Add to Dock"** (Safari Mac) functionality. Running as a standalone PWA removes the URL bar, preventing scanner input leakage.

---

## 📦 Professional Deployment & Offline Usage
While INFOBRIDGE Reader is "offline-first," it is built as a **Progressive Web App (PWA)** to leverage modern browser security. Installing as a standalone app removes the URL bar, preventing scanner input leakage into the browser's address history.

> [!CAUTION]
> **Protocol Restriction**
> Opening `index.html` directly via the `file://` protocol is **not recommended** for professional use. Browser security policies disable Service Workers and PWA installation on the file protocol, rendering the tool vulnerable to scanner input leakage into the URL bar.

### Deployment Options for Air-Gapped Environments

For secure, air-gapped workstations, choose one of the following institutional deployment methods:

#### A. Institutional Intranet Server (Recommended)
The most robust approach for medical and government facilities is to host the extracted ZIP contents on an **Internal Web Server** or **Institutional Intranet**.
- **Requirement**: Serve via `https://` (or `http://` for trusted intranet domains).
- **Benefit**: Enables centralized updates and allows all authorized workstations to "Install" the Reader as a native-feeling standalone app.

#### B. Local Secure Context (Workstation Level)
If a central server is unavailable, you must create a local secure context on the workstation:
1. **Download**: Visit the [Latest Releases page](https://github.com/icommuse/INFOBRIDGE-Reader/releases/latest) and download the file named **INFOBRIDGE-Reader-vX.X.X.zip** (the pre-built distribution package). Extract this ZIP to a local directory.
2. **Launch Local Host**: Initialize a local listener (requires Python or similar runtime):
   ```bash
   cd /path/to/INFOBRIDGE-Reader
   python3 -m http.server 8080
   ```
3. **Access**: Navigate to `http://localhost:8080/index.html`.
4. **Finalize**: Use the browser's **"Install App"** feature to move the utility into its own secure, URL-bar-free application window.


Refer to [README.txt](./README.txt) (or [TROUBLESHOOTING.txt](./TROUBLESHOOTING.txt)) for detailed installation steps.

---

## 🛰️ Security & Technical Reliability

Designed for high-security environments (Medical, Legal, Government), INFOBRIDGE Reader prioritizes transparency and verifiable data integrity.

- **AES-256-GCM Standard Encryption**: All "Secret" mode packets are decrypted entirely within the browser's local memory (WebCrypto API) and are never persisted or uploaded.
- **True Air-Gap Data Transfer**: Data is transmitted via optical signals (QR codes), ensuring physical isolation from the host network and mitigating risks associated with network-based exploits.
- **External API Isolation**: Base64 assembly and cryptographic processing run locally in the browser, and the decode/decrypt pipeline does not call external APIs.
- **Verifiable Protocols**: The system utilizes standard Base64 encoding and audited cryptographic methods, allowing for external security audits.
- **Zero-Installation Footprint**: Operates as a portable web tool requiring no drivers, reducing the attack surface on restricted workstations.

---

## 🛡️ Hardening & HID Security

Handheld QR scanners are recognized by the OS as "Keyboards" (HID). While this ensures compatibility, it introduces a potential vector for **HID Keystroke Injection attacks**.

### Multi-Layered Defense
1. **Software Validation**: The Reader automatically inspects and blocks illegal ASCII control characters (e.g., Tab, Escape, GUI commands) to abort malicious injections.
2. **User Responsibility**: Ensure your scanner hardware is configured to **disable the transmission of control characters**. Enterprise scanners provide barcodes to enforce "Printable ASCII only."

> [!IMPORTANT]
> **User Responsibility: Scanner Hardware Configuration**
> To mitigate OS-level risks, ensure your scanner hardware is configured to **disable the transmission of control characters and GUI/Modifier keys**. Most enterprise scanners provide configuration barcodes to enforce "Printable ASCII only" transmission.

---

## 🔒 For Security Officers: Technical Assurance

We understand the rigorous requirements of managed air-gap environments. This Reader satisfies the following criteria:

### 1. Zero-Footprint & Sandboxed Execution
*   **No Registry Impact**: Standalone HTML file. No installer and no traces in host OS registry.
*   **Browser Sandbox**: Operates entirely within the browser's security sandbox.

### 2. Verified Network Isolation
*   **Offline-Ready**: After loading, core scan, assembly, and decryption functions are designed to operate without internet connectivity.
*   **No Callbacks**: Contains no network hooks, telemetry, or external API calls.

### 3. Transparent & Auditable Logic
*   **Human-Readable Source**: Logic is implemented in plain, unminified JavaScript.
*   **Static Logic**: No dynamic script loading. Security teams can perform a full source code audit via any text editor.

---

## ⚖️ License & Copyright
Copyright © 2026 Koichiro Watanabe. All rights reserved.
Distributed under the **Apache License 2.0**.
