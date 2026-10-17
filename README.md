# 🎬 Blender2Three-MCP — Unified Visual Parity Showcase

[![Visual Parity](https://img.shields.io/badge/Parity%20Delta-67.3%25%20→%2095.5%25-10b981?style=for-the-badge&logo=blender&logoColor=white)](https://oh-codes.github.io/Blender2Threejs-MCP/Showcases/)
[![Draco Compression](https://img.shields.io/badge/Draco%20Savings-Up%20to%2095.2%25-f59e0b?style=for-the-badge&logo=webgl&logoColor=white)](https://oh-codes.github.io/Blender2Threejs-MCP/Showcases/)
[![Three.js](https://img.shields.io/badge/Three.js-r170%20AgX-38bdf8?style=for-the-badge&logo=threedotjs&logoColor=white)](https://threejs.org/)
[![License](https://img.shields.io/badge/License-MIT-818cf8?style=for-the-badge)](LICENSE)

> **Automated pipeline translating complex Blender Cycles scenes into high-performance Three.js WebGL with sub-pixel visual parity, AgX color management, and production Draco/Meshopt compression.**

👉 **[Launch Interactive Live Showcase Portal](https://oh-codes.github.io/Blender2Threejs-MCP/Showcases/)** — *Interactive split-slider comparing Blender Cycles reference stills with real-time Three.js WebGL at 60 FPS.*

---

## 🌟 Why Blender2Three-MCP?

Exporting 3D assets from Blender to real-time WebGL engines has traditionally been plagued by severe fidelity loss:

1. **Procedural Shaders Don't Export**: Procedural noise, Voronoi textures, and complex node graphs collapse to plain grey materials in stock glTF.
2. **Transmission & Volume Mismatch**: Refractive glass turns opaque black or loses absorption depth without manual shader hacking.
3. **Color Space Mismatches**: Linear sRGB clamping crushes specular highlights and washes out subtle shadows compared to Blender's native AgX tone mapper.
4. **Armature & Rig Distortion**: Character joints often experience vertex tearing or unnatural bending due to non-rigid automatic bone weights.
5. **Huge Asset Files**: Uncompressed production assets routinely exceed 50 MB, making them unusable for web delivery.

**Blender2Three-MCP bridges this gap completely through an automated multi-worker pipeline.**

---

## 📊 Comprehensive Parity & Bandwidth Benchmarks

All metrics verified using Mean Absolute Error (MAE) comparing 1080p ground-truth Blender Cycles AgX renders against real-time Three.js WebGL frames:
`Visual Parity Score (%) = 100.00% - Full Frame MAE (%)`.

| # | Showcase Lab | Stock OOB Parity | Full Pipeline Parity | Full MAE | Foreground MAE | Raw GLTF Size | Draco / Meshopt | Bandwidth Reduction |
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **00** | **🎬 Bender Grand Finale Cinematic** | 82.00% | **95.23%** | 4.77% | 11.38% | **51.65 MB** | **2.49 MB** | **-95.17%** |
| **01** | **💎 Optical Glass & Volume Lab** | 72.40% | **95.80%** | 4.20% | 8.15% | 1.84 MB | **412 KB** | **-77.61%** |
| **02** | **🪞 Advanced PBR & Mirrors Lab** | 76.50% | **96.40%** | 3.60% | 4.76% | 2.15 MB | **480 KB** | **-77.67%** |
| **03** | **💡 Punctual Lights & Soft Shadows** | 81.20% | **94.70%** | 5.30% | 4.98% | 940 KB | **215 KB** | **-77.13%** |
| **04** | **🎨 Procedural Shader Baking Lab** | 10.40% | **95.10%** | 4.90% | 17.57% | 14.80 MB | **1.85 MB** | **-87.50%** |
| **05** | **⚙️ Rigging & Gyro Hierarchy Lab** | 83.80% | **96.20%** | 3.80% | 16.63% | 1.79 MB | **264 KB** | **-85.28%** |
| **06** | **🫧 Iridescence Soap Bubbles Lab** | 68.20% | **95.50%** | 4.50% | 6.56% | 3.42 MB | **684 KB** | **-80.00%** |
| **07** | **🌿 Alpha Foliage & Decals Lab** | 74.30% | **94.80%** | 5.20% | 5.26% | 4.88 MB | **890 KB** | **-81.76%** |
| **08** | **⚡ GPU Instancing Array Lab** | 79.10% | **97.10%** | 2.90% | 8.80% | 8.92 MB | **1.03 MB** | **-88.45%** |
| **09** | **🖥️ Multi-Material Cyber Deck Lab** | 81.00% | **95.90%** | 4.10% | 24.16% | 72.3 KB | **16.6 KB** | **-77.02%** |
| **10** | **🗼 Sentinel Tower LOD Lab** | 78.20% | **95.10%** | 4.90% | 13.77% | 376.5 KB | **122.7 KB** | **-67.41%** |
| **11** | **⛪ Gothic Sacred Relic Volumetric** | 14.80% | **93.30%** | 6.70% | 6.68% | 1.12 MB | **260 KB** | **-76.79%** |
| **12** | **🤖 Articulated Mecha Sentinel Lab** | 75.10% | **96.20%** | 3.80% | 3.49% | 243 KB | **52.6 KB** | **-78.33%** |
| **13** | **🧪 Magnetic Containment Goo Lab** | 65.10% | **96.04%** | 3.96% | 6.27% | 439.8 KB | **323.7 KB** | **-26.40%** |
| **AVG** | **Full Benchmark Suite Overall** | **67.29%** | **95.53%** | **4.47%** | **9.89%** | **90.9 MB** | **8.88 MB** | **-90.23%** |

---

## 🔬 Core Architecture & Pipeline

```
Blender Cycles Scene (.blend)
     │
     ├──► 1. Geometry Worker
     │       └── Decimation, Clean Normals, Multi-Primitive Partitioning, UV Generation
     │
     ├──► 2. Material Preprocessor
     │       └── Headless 4-Pass Cycles Shader Baker (BaseColor, Normal, ORM Packed)
     │
     ├──► 3. Optical Patcher
     │       └── KHR glTF Extensions (Transmission, Volume, Iridescence, Clearcoat, Sheen)
     │
     └──► 4. Production Exporter & Compressor
             └── Draco / Meshopt Geometry Compression (up to 95.2% size reduction)
                     │
                     ▼
             Three.js WebGL Runtime (AgXToneMapping, PCF Shadows, 60 FPS)
```

### Highlights of Technical Solves:
- **Rigid Bone Weighting Engine**: Ensures hard mechanical bodies (visors, doors, antennas) do not stretch or tear during skeletal animations.
- **Dynamic Transmission & Volume**: Injects physical `KHR_materials_volume` thickness attenuation distance and absorption colors for crystal glass and fluids.
- **Thin-Film Iridescence**: Emulates wave interference on soap bubbles (`KHR_materials_iridescence`) across varying nanometer thickness.
- **Draco & Meshopt Compression**: Drastically cuts download times and memory overhead while preserving vertex attributes (morph targets, dual UV sets).

---

## 🚀 Running the Showcase Locally

Clone the repository and serve with any local HTTP server:

```bash
# Clone the repository
git clone https://github.com/oh-codes/Blender2Threejs-MCP.git
cd Blender2Threejs-MCP

# Run with Python
python -m http.server 8080

# Or run with Node.js
npx serve .
```

Open `http://localhost:8080/Showcases/` in your browser.

### Adding or removing a showcase

The portal discovers its content from `Showcases/showcases.json`; `index.html` contains no scene
list. To add a showcase:

1. Create `Showcases/assets/<id>/` with `<id>.glb`, `cycles_reference.png`, `oob_reference.png`,
   and `pass1_reference.png`.
2. Add one object to the `showcases` array in `Showcases/showcases.json`. `id` is the only required
   field; asset paths default to the folder convention above.
3. `bender_cinematic` is pinned first and selected by default (`default` in the manifest); the rest
   render in manifest order.

Removing a showcase means deleting its JSON entry (and optionally the asset folder). Static hosting
cannot enumerate directories in the browser, so the JSON is the discovery layer; a generator can
emit it from the folder tree later.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
