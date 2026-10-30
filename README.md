# Local Verification (`LV`) Mobile App

<p align="center">
  <strong>100% Air-Gapped · On-Device AI · Zero-Knowledge Chastity Verification</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Network-Air--Gapped%20(No%20Internet)-10B981?style=for-the-badge&logo=shield" alt="Air-Gapped" />
  <img src="https://img.shields.io/badge/Security-HMAC--SHA256%20Proof-7C3AED?style=for-the-badge&logo=auth0" alt="HMAC Proof" />
  <img src="https://img.shields.io/badge/AI%20Engine-On--Device%20LiteRT-6366F1?style=for-the-badge&logo=tensorflow" alt="LiteRT" />
  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS-white?style=for-the-badge&logo=flutter" alt="Flutter" />
</p>

---

## 🛡️ The Zero-Knowledge Privacy Guarantee

**Local Verification (`LV`)** is an offline verification utility designed for wearers of chastity devices who want absolute certainty that **no intimate photos or videos can ever be stored, leaked, or sent to a server**.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        YOUR MOBILE DEVICE                              │
│                                                                        │
│   [Camera] ──> [RAM Buffer] ──> [On-Device AI] ──> [Scrub RAM (0x00)]   │
│                                       │                                │
│                                       ▼                                │
│                     [Signed HMAC-SHA256 Proof Token]                   │
│                                       │                                │
│                     [High-Contrast QR Code on Screen]                  │
└───────────────────────────────────────┬────────────────────────────────┘
                                        │ (Scan with Web Cam)
                                        ▼
                           ┌─────────────────────────┐
                           │    Chaster Extension    │
                           │  (Verifies Cryptography)│
                           └─────────────────────────┘
                               NO IMAGES TRANSMITTED
```

### 1. 📴 Physically Air-Gapped (Zero Internet Permission)
The app is built without network capability. In the application manifest (`AndroidManifest.xml`), the standard `android.permission.INTERNET` permission is **completely absent**. The mobile operating system physically blocks the app from creating any socket, HTTP, or network connections.

### 2. 🧼 Ephemeral RAM Scrubbing
Camera frames reside strictly in temporary volatile memory (`Uint8List`). Immediately upon running on-device inference, the memory buffer is actively overwritten with all zeros (`0x00`) using cryptographic memory scrubbing before being deallocated.

### 3. 🚫 Zero Local Storage
The app does **not** save photos to the camera roll, photo gallery, temporary cache, or filesystem. Even if verification fails or an inspection error occurs, no imagery is ever persisted.

### 4. 🔐 Cryptographic Proofs
Rather than uploading pictures, `LV` generates a tamper-proof **HMAC-SHA256 signed attestation token**. The Chaster browser extension validates the mathematical signature using your shared secret key without ever needing to see your photos.

---

## 📥 Download & Installation

Official pre-built, signed release APKs are hosted directly in the [Releases](https://github.com/osepam-netizen/lv/releases) section of this repository.

### Quick Install (Android)
1. Navigate to [Releases](https://github.com/osepam-netizen/lv/releases/latest).
2. Download the latest `lv-vX.Y.Z.apk`.
3. Open the downloaded file on your Android device and confirm installation.

### Verify SHA-256 Checksum (Optional)
To verify the integrity of the downloaded file:
```bash
# On macOS / Linux:
sha256sum lv-vX.Y.Z.apk
# Compare the output with lv-vX.Y.Z.apk.sha256 from the release page
```

### Verify No Internet Permission (Audit)
You can independently inspect the APK to confirm that zero internet permissions are requested:
```bash
aapt dump permissions lv-vX.Y.Z.apk
# Output will confirm that android.permission.INTERNET is NOT present!
```

---

## 🚀 Step-by-Step User Guide

### Step 1: Pair with Your Chaster Extension (One-Time)
1. Open the Chaster browser extension on your computer.
2. In the extension settings, click **Show Pairing Key**.
3. In the LV mobile app, tap the **Key** icon on the top bar and scan the extension QR code (or paste the secret key).
4. Both devices now share a cryptographic HMAC key used to sign and verify proofs offline.

### Step 2: Receive Your Dynamic Challenge Code
When a check-in is required, the Chaster extension displays a unique 6-digit challenge code (e.g., `849 201`).
Write this code on a piece of paper or your skin.

### Step 3: Run On-Device Verification
1. On the **Verify** tab, select your device profile:
   - **Male Chastity Cage** (Built-in Lock or Padlock)
   - **Female Chastity Belt** (Built-in Lock or Padlock)
2. Tap **Start Camera Inspection**.
3. Align the camera viewfinder so that your locked device and handwritten challenge code are clearly visible.
4. Tap the capture button.

### Step 4: Live On-Device Analysis
The app switches to the analyzing screen with an animated status indicator:
1. *Scans for handwritten 6-digit challenge code via local OCR.*
2. *Inspects device presence and locked padlock engagement via LiteRT neural engine.*
3. *Enforces anti-cheating rules (detects open shackles, inserted keys, or unworn loose hardware).*
4. *Signs the verification result with HMAC-SHA256.*

### Step 5: Submit Verification to Chaster
Once verified, the full-screen **Verification Result** page displays:
- A high-contrast **Chaster Pairing QR Code**.
- An **HMAC-SHA256 Proof Token**.

**To submit:**
- Hold your phone screen up to your computer webcam to scan the QR code in the Chaster extension, **OR**
- Tap **Copy** and paste the proof token string into the Chaster extension input field.

The Chaster extension verifies the signature instantly and clears your verification check-in!

---

## 🔍 Hardware & Compliance Profiles

| Device Profile | Required Objects | Prohibited Violations |
| :--- | :--- | :--- |
| **Male Cage (Padlock)** | `cage_worn` + `padlock_locked` | `padlock_open`, `key_inserted`, `device_not_worn`, unlocked anatomy |
| **Male Cage (Built-in)** | `cage_worn` (locked cylinder) | `key_inserted`, `device_not_worn`, unlocked anatomy |
| **Female Belt (Padlock)** | `belt_worn` + `padlock_locked` | `padlock_open`, `key_inserted`, `device_not_worn`, unlocked anatomy |
| **Female Belt (Built-in)** | `belt_worn` | `key_inserted`, `device_not_worn`, unlocked anatomy |

---

## ❓ Frequently Asked Questions (FAQ)

<details>
<summary><strong>Can keyholders or Chaster admins see my verification photos?</strong></summary>

**No. It is technically impossible.** The application has no internet access, and images are scrubbed from RAM immediately after analysis. Neither Chaster, nor your keyholder, nor the app developers receive any images.
</details>

<details>
<summary><strong>What if my challenge code isn't recognized?</strong></summary>

Make sure the code is written in bold, clear numbers with high contrast (dark marker on white paper or skin) and that your room is well lit.
</details>

<details>
<summary><strong>Can this be used on an airplane or without cellular service?</strong></summary>

**Yes!** The application is 100% offline. You do not need Wi-Fi or cellular reception to run verification.
</details>

---

## 📜 License

Licensed under the MIT License. See [LICENSE](LICENSE) for details.
