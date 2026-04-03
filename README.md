# 🔍 Detective Clue Hunter — Image Processing Game

An interactive desktop game built with Python that challenges players to restore and decode distorted images using real image processing techniques. Each level hides a clue inside a corrupted image; the player must apply the right filters and enhancements to uncover it.

---

## 📁 Project Files

| File | Description |
|---|---|
| `image_detective_game.py` | Original 5-level version of the game |
| `improved_detective_game.py` | Enhanced 3-level version with scrollable tools, undo history, frequency decoding, and a linked narrative |

---

## 🎮 Gameplay Overview

The game presents the player with a distorted image and a mission question. The player uses image processing tools — noise reduction, sharpening, enhancement, and frequency-domain decoding — to restore the image and find the hidden answer (an ID number, a code, or a date). Each correct answer earns 100 points.

### Levels (Improved Version)

| Level | Distortion Applied | Objective | Recommended Tool |
|---|---|---|---|
| 1 | Gaussian blur + darkening | Find the ID number | Unsharp Masking / Laplacian Sharpen |
| 2 | Salt & pepper noise + darkening | Find the secret codes | Median Filter + Contrast Stretch |
| 3 | Frequency-domain encoding (phase scramble + permutation) | Decode image using keys from Level 2 | Decode Image with Keys |

The levels are intentionally linked: the codes discovered in Level 2 are the decryption keys needed to unlock Level 3.

---

## 🛠️ Requirements

- Python 3.8+
- OpenCV (`cv2`)
- NumPy
- Pillow (`PIL`)
- Tkinter (included with standard Python on Windows/Linux)

Install dependencies:

```bash
pip install opencv-python numpy Pillow
```

---

## 🚀 Running the Game

Run the improved version:

```bash
python improved_detective_game.py
```

Or the original version:

```bash
python image_detective_game.py
```

---

## 🖼️ Image Assets

Place your clue images in the path specified in `preload_clue_images()`. By default, the improved version expects:

```
e:/Image Processing Lab/Project/Clue-1.png
e:/Image Processing Lab/Project/Clue-2e.png
e:/Image Processing Lab/Project/Clue-3.png
```

Update these paths to match your local setup before running the game.

> **Note for Level 3:** The third image must be grayscale, as the frequency-domain encoding operates on a single channel.

---

## 🧰 Image Processing Tools Available

### Noise Reduction
- **Gaussian Filter** — smooths Gaussian noise; kernel size and sigma are adjustable via sliders
- **Median Filter** — effective for salt & pepper noise; kernel size is adjustable
- **Bilateral Filter** — edge-preserving smoothing

### Sharpening
- **Unsharp Masking** — subtracts a blurred copy to enhance edges
- **Laplacian Sharpen** — applies a 3×3 Laplacian kernel for edge enhancement

### Enhancement
- **Gamma Correction** — adjustable gamma for brightness/contrast control
- **Contrast Stretch** — linear scaling with adjustable alpha and beta values

### Frequency Domain
- **Decode Image with Keys** — prompts for a phase key and permutation key, then reverses the frequency-domain encoding applied in Level 3
- **Auto Notch Filter** — detects and removes periodic noise using Butterworth notch reject filters

### Controls
- **Undo Last Action** — steps back through up to 10 previous states
- **Reset Image** — reverts to the distorted starting image
- **Get Hint** — displays a level-specific hint
- **Submit Answer** — opens a dialog to enter the discovered clue

---

## 🔐 Frequency Encoding (Level 3)

Level 3 uses a two-stage encoding scheme applied in the frequency domain:

1. **Phase scrambling** — a random phase mask (seeded with `key_phase`) is added to the FFT phase spectrum
2. **Coefficient permutation** — the complex Fourier coefficients are shuffled using a seeded random permutation (`key_perm`)

Decoding reverses both steps in the correct order. Using the wrong keys produces a meaningless result, so the player must find both codes in Level 2 before proceeding.

---

## 📊 Scoring

Each level is worth **100 points** for a correct answer, regardless of how many processing steps were applied. Incorrect answers award no points but still advance the level. Maximum possible score: **300 points**.

The original `image_detective_game.py` uses **PSNR** (Peak Signal-to-Noise Ratio) to score image quality rather than a text answer:

| PSNR | Points |
|---|---|
| > 25 dB | 100 |
| > 20 dB | 80 |
| > 15 dB | 60 |
| ≤ 15 dB | 20 |

---

## 🗂️ Code Structure

```
ImageDetectiveGame / ClueHuntingGame
├── __init__()               # Game state initialisation
├── create_widgets()         # Build the Tkinter UI
├── preload_clue_images()    # Load and resize clue images from disk
├── load_level()             # Set up the current level
├── apply_hiding_distortion()# Apply the level-specific distortion
├── display_image()          # Render current image on canvas
├── save_to_history()        # Push state to undo stack
├── apply_*()                # Individual image processing methods
├── encode_image_frequency() # Level 3 encoding
├── decode_image_frequency() # Level 3 decoding
├── apply_notch_filter()     # Automatic periodic noise removal
├── submit_answer()          # Validate player answer and advance level
└── end_game()               # Reset all state
```

---

## 📝 Notes

- The game window is **1200×800** pixels; a larger monitor is recommended.
- The tools panel in the improved version is scrollable to accommodate all controls.
- Both files are self-contained and can be run independently.
