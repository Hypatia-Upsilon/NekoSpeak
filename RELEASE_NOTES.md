# Release v1.4.1 - ARMv7 Compatibility Fix 🔧

## 🐛 Bug Fixes

### Fixed SIGBUS Crash on 32-bit ARMv7 Devices
- **Issue**: Native crash (SIGBUS, BUS_ADRALN) when loading INT8 ONNX models on 32-bit ARM devices
- **Root Cause**: Memory-mapped model loading caused unaligned memory access on ARMv7
- **Fix**: Architecture-aware model loading that uses byte array loading on 32-bit ARM to bypass mmap alignment issues
- **Impact**: INT8 models now load correctly on older 32-bit Android devices

See [ADR-005](docs/adr/ADR-005-sigbus-fix-armv7-int8-models.md) for technical details.

## 📥 Downloads

| Variant | Description |
|---------|-------------|
| app-universal-release.apk | Works on all devices |
| app-arm64-v8a-release.apk | Modern 64-bit devices |
| app-armeabi-v7a-release.apk | Older 32-bit devices (now fixed!) |

## 💡 Upgrade Notes
Drop-in upgrade from v1.4.0. No data migration required.

---

# Release v1.4.0 - Previous Release

(Previous release notes below)

---

# Release v1.2.0 - Adaptive Streaming Engine 🚀

## ⭐ What's New

### Adaptive Streaming Engine
- **Self-tuning buffer management** - Automatically measures device performance and adjusts buffer sizes
- **Parallel generation & decoding** - Uses Kotlin coroutines to run generation and decoding concurrently
- **Real-time performance tracking** - Monitors frame generation times and calculates optimal settings
- **Zero configuration required** - Works optimally on any device without manual tuning

### Performance Improvements
- **Reduced startup latency** - Faster devices get smaller initial buffers for quicker audio start
- **Smoother long-form playback** - Better buffer management prevents audio gaps on slower devices
- **Thermal throttling handling** - Continuously re-measures performance every 10 frames

### Documentation
- **Updated README** - Added detailed documentation of the adaptive streaming architecture
- **Mermaid diagrams** - Visual explanation of the parallel streaming pipeline

## 📥 Downloads

| Variant | Size | Description |
|---------|------|-------------|
| [app-universal-release.apk](https://github.com/siva-sub/NekoSpeak/releases/download/v1.2.0/app-universal-release.apk) | ~135MB | Works on all devices |
| [app-arm64-v8a-release.apk](https://github.com/siva-sub/NekoSpeak/releases/download/v1.2.0/app-arm64-v8a-release.apk) | ~88MB | Modern devices (Pixel, Samsung S-series) |
| [app-armeabi-v7a-release.apk](https://github.com/siva-sub/NekoSpeak/releases/download/v1.2.0/app-armeabi-v7a-release.apk) | ~82MB | Older/low-end devices |

## 🔧 Technical Details

### How Adaptive Streaming Works

1. **Performance Measurement**: Tracks average frame generation time (e.g., 117ms per frame)
2. **Ratio Calculation**: Compares generation speed vs playback speed (80ms per frame = 1.46 ratio)
3. **Buffer Tuning**: Adjusts initial buffer (8-30 frames), decode threshold, and reserve based on ratio

| Device Speed | Ratio | Initial Buffer | Decode Threshold |
|--------------|-------|----------------|------------------|
| Fast (realtime+) | ≤1.0 | 8 frames | 3 |
| Medium | ≤1.5 | 15 frames | 8 |
| Slow | >1.5 | 20+ frames | 10 |

### Known Limitations
- Very long sentences may still experience some choppiness on slower devices
- Runs on CPU-only ONNX inference (NNAPI/QNN not supported for transformer models)
- For smoothest experience on long texts, use Batch mode in settings

## 💡 Upgrade Notes
This is a drop-in upgrade from v1.1.0. No data migration required.

---
**Full Changelog**: [v1.1.0...v1.2.0](https://github.com/siva-sub/NekoSpeak/compare/v1.1.0...v1.2.0)
