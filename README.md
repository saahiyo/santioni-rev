# Santioni Spirits ("The Notturno Experience") — Reverse-Engineered WebGL Architecture Study

> ⚠️ **Disclaimer & Educational Case Study**  
> This repository is an **independent, non-commercial technical reverse-engineering study** exploring client-side WebGL architecture, multi-threaded Web Workers, and GPU asset pipelines.  
> 
> **Original Work & Credits:**
> - **Original Experience:** [santionispirits.com](https://santionispirits.com/)
> - **Creative Development & Engine:** [Active Theory](https://activetheory.net/) (built on their proprietary **Hydra 3D WebGL Engine**)
> - **Brand & Production:** Santioni Spirits
> - **Accolades:** Awwwards Site of the Day (SOTD), FWA of the Month (August 2026), FWA of the Day.
> 
> All visual art, audio stems, 3D character designs, trademarks, and story content remain the sole intellectual property of **Santioni Spirits** and **Active Theory**.

---

## 🔬 Project Overview

In the creative web development community, Active Theory’s in-house **Hydra WebGL engine** is revered as a closed-source "black box". The production application ships with an empty initial DOM shell, relying on dynamic client-side bootstrapping, obfuscated JavaScript ASTs, multi-threaded Web Workers, and binary geometry streaming.

This project represents a **full standalone reconstruction** of "The Notturno Experience", successfully running the complete interactive 3D WebGL narrative, shader pipelines, spatial audio, and character animation loops completely offline and decoupled from external origins.

---

## 🛠️ Reverse Engineering Methodology & Skills Used

This reconstruction was executed using **REA (Reverse Engineer Anything)** alongside automated agent analysis pipelines:

### 1. Bundle Deconstruction via REA
* Utilized REA's JavaScript application analysis pipeline (`analyze-javascript-application`) to parse the Abstract Syntax Tree (AST) of the minified 2.1 MB bundle (`app.1782836328290.js`).
* Traced dynamic module imports, runtime dispatchers, and state machine hooks embedded within client-side state payloads (`uil.*.json`).

### 2. Multi-threaded Worker & WASM Resolution
* **Web Worker Threading:** Extracted and satisfied runtime worker dependencies (`hydra-thread.js`) responsible for offloading geometry computation from the main thread.
* **3D Geometry Decoders:** Sourced and configured Google Draco WebAssembly wrappers (`draco_decoder.wasm` and `draco_wasm_wrapper.js`) to parse compressed 3D meshes without engine fallback errors.
* **GPU Texture Transcoding:** Resolved Basis Universal transcoders (`basis_transcoder.wasm`) for native KTX2/Basis GPU compression.

### 3. WebGL Shader & Binary Mesh Extraction
* **Shader Pipeline:** Harvested dynamic GLSL vertex and fragment pipelines (`compiled.vs`, `AntiGravityFloorShader`, `ApproachBackgroundShader`, `BorderShader`, noise distortion, and fluid edge filters).
* **3D Binary Geometries (`.bin`):** Extracted all 23 custom binary meshes and skeletal animation rigs (e.g. `saint-pour-bottle2.bin`, `floating-frame-walk.bin`, `saint-antigravity.bin`, `arm-skin.bin`).

### 4. Spatial Audio & Voiceover Synchronization
* Extracted all 47 interactive soundscapes:
  * 16 sequential voiceover narrative tracks (`assets/audio/vo/1.mp3` – `16.mp3`, `orange.mp3`, `marshmallow.mp3`).
  * Ambient background audio (`wind.ogg`, `jazz.ogg`).
  * Tactile UI & interaction sound cues (drones, bottle clinks, levitation hums, portal transitions).

---

## 📁 Repository Structure

```
santionispirits/
├── index.html                   # Standalone WebGL entry point with cached HydraX state
├── vercel.json                  # Edge deployment routing and MIME config
├── assets/
│   ├── data/                    # HydraX CMS state and shader parameter maps
│   ├── js/                      # Core WebGL runtime, worker threads, and WASM decoders
│   │   ├── hydra/               # Background web worker threads (hydra-thread.js)
│   │   └── lib/                 # Draco WASM decoders and Basis transcoders
│   ├── geometry/                # 23 binary 3D meshes & character animation rigs (.bin)
│   ├── shaders/                 # Compiled WebGL vertex and fragment shaders
│   ├── audio/                   # 16 narrative VO tracks, ambient stems, and UI soundscapes
│   ├── images/                  # High-resolution WebGL textures, noise maps, and stills
│   ├── fonts/                   # Typography (PP Nikkei Maru, GT Era, Charles Rosie)
│   ├── favicon/                 # SVG brand favicon
│   └── social/                  # OpenGraph preview assets
```

---

## 🚀 How to Run Locally

Because the application streams binary mesh data (`.bin`), WebAssembly (`.wasm`), and GLSL shaders via client-side `fetch()`, browsers enforce CORS security when opened via `file://`.

Run a local HTTP server:

```powershell
cd santionispirits
python -m http.server 8090
```

Open your browser at: **http://localhost:8090**  
Click **YES** on the Age Gate to enter the interactive 3D WebGL story.

---

## 📜 Legal Notice
This project is shared strictly for educational and analytical purposes under fair use. If you are a copyright holder and wish for any asset to be removed, please open an issue and it will be addressed immediately.
